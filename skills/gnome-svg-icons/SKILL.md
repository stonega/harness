---
name: gnome-svg-icons
description: Add, package, and troubleshoot custom SVG icons in native GNOME applications using GTK 4, libadwaita, and GJS. Use for bundled toolbar and menu icons, symbolic recoloring, light/dark/high-contrast behavior, preserved multicolor artwork, and icons missing from installed builds. GNOME Shell St widgets use a different styling API.
---

# Custom SVG Icons in GNOME Apps

Implement custom artwork through the host application's existing icon helper and packaging conventions. Preserve the supplied SVG geometry and intended palette. A theme-color bug should be fixed in loading or recoloring, not by substituting different artwork.

This guide targets GTK 4/libadwaita with GJS examples and does not assume a particular application or repository layout. Adapt bindings for other languages. Distinguish GTK's native symbolic rendering from explicit SVG recoloring; they have different color and lifecycle behavior.

## Choose the Rendering Path

Inspect the existing helper, a call site, SVG files, resource manifest, install rules, and icon tests before editing. Check the supported GTK/librsvg versions when relying on newer SVG symbolic features.

| Requirement | Rendering path |
| --- | --- |
| Monochrome controls following widget foreground, including suggested/destructive states | Native named symbolic icon through `Gtk.IconTheme` and `Gtk.Image` |
| A reproduced platform issue with native SVG recoloring | Use a shared helper to explicitly recolor and rerender on appearance changes |
| Logo or artwork with fixed brand colors | Regular SVG through a file icon or paintable; preserve its colors |
| Mixed fixed accents and theme-dependent foreground | Explicitly recolor only the designated foreground token |

An arbitrary multicolor palette is not equivalent to GTK's semantic success/warning/error palette. Do not force brand artwork through the symbolic pipeline. Inspect the SVG and its intended appearance instead of classifying artwork by filename alone.

## Prepare the Asset

- Preserve the `viewBox`, proportions, paths, license, and attribution. Use self-contained SVGs without external fonts or linked images.
- Use `-symbolic.svg` for native symbolic icons. Load the icon name without `.svg`.
- When explicitly recoloring controlled assets, define one consistent foreground token and reserve it for parts that should change with the theme. Literal replacement requires an exact match; equivalent hex or RGB spellings will not match automatically.
- Avoid assuming that a standalone SVG inherits the enclosing widget's `currentColor` as it would in a browser. Verify the selected GTK/librsvg loading path; use explicit foreground values before rasterization when compatibility requires it.
- Preserve fixed accent colors and keep them separate from any replaceable foreground token.
- For native symbolic icons, keep artwork within the supported GTK symbolic SVG subset. For older target versions, convert unsupported strokes or shapes to filled paths without changing their appearance.

## Native Symbolic Icons

When the host app already uses registered resources and named icons, retain that approach. Put symbolic resources under themed size/context directories. GTK's migration guide warns that icons directly under the icon resource root are unthemed and may not recolor.

For example, bundle `data/resources/send-symbolic.svg` using this manifest, compiled with `data` as its source directory:

```xml
<gresources>
  <gresource prefix="/org/example/App/icons">
    <file alias="scalable/actions/example-send-symbolic.svg">resources/send-symbolic.svg</file>
  </gresource>
</gresources>
```

Register an external compiled bundle once during application startup, after GTK has a display. Replace the example prefix with the application's actual prefix:

```javascript
import Gdk from 'gi://Gdk?version=4.0';
import Gio from 'gi://Gio?version=2.0';
import Gtk from 'gi://Gtk?version=4.0';

function registerBundledIcons(bundlePath) {
    const resource = Gio.Resource.load(bundlePath);
    Gio.resources_register(resource);
    const theme = Gtk.IconTheme.get_for_display(Gdk.Display.get_default());
    theme.add_resource_path('/org/example/App/icons');
}

function createSendButton() {
    return new Gtk.Button({
        tooltip_text: 'Send',
        child: new Gtk.Image({
            icon_name: 'example-send-symbolic',
            pixel_size: 16,
        }),
    });
}
```

Pass `registerBundledIcons` the build-configured bundle path before creating the button. Do not register the same bundle repeatedly if the application already owns its registration. `Gtk.Application` automatically adds the `icons` directory under its configured resource base path; explicit `add_resource_path` is useful for a different prefix. Compiling a bundle alone does not register it.

Give icon-only controls an accessible name and tooltip. Use the button's `child` for custom images; assigning `icon_name` afterward replaces that content. A system icon is a fallback for a missing or unreadable asset, not evidence that the custom SVG loaded successfully.

For loose files, register a correctly structured icon theme search directory using `Gtk.IconTheme.add_search_path`, or follow the project's existing file-icon approach. Do not assume a flat arbitrary asset directory has symbolic theme semantics. Confirm the resolved icon is the bundled asset, especially when its name also exists in the system theme.

Named symbolic images can inherit GTK widget foreground colors. A `Gdk.Texture` has already rasterized pixels: setting CSS `color` on its `Gtk.Image` does not automatically recolor them. A `Gio.FileIcon` describes a file, and is not by itself a guarantee of symbolic rendering. Renaming an SVG or forcing symbolic lookup does not repair malformed paths or SVG data.

## Fixed-Color SVGs

Keep logos and multicolor artwork on a regular-image path. For example, after resolving a packaged file path, use the `Gio` and `Gtk` imports above:

```javascript
function createLogoImage(iconPath, pixelSize = 24) {
    return new Gtk.Image({
        gicon: new Gio.FileIcon({ file: Gio.File.new_for_path(iconPath) }),
        pixel_size: pixelSize,
    });
}
```

Use an ordinary `.svg` filename for fixed-color artwork and avoid forcing symbolic styling. Handle missing or unreadable assets in the shared loader. Test the actual palette on both light and dark backgrounds; use designer-provided variants when the artwork requires them.

## Explicit Recoloring When Needed

When native symbolic rendering does not meet a verified compatibility or mixed-palette requirement, keep the workaround in a shared helper with this pipeline:

1. Resolve the installed/source asset path and read the original SVG.
2. Determine effective appearance using `Adw.StyleManager.get_dark()` and `get_high_contrast()`. The requested `color_scheme` is not the effective `dark` state.
3. Replace only the controlled foreground token; retain all other colors.
4. Decode the resulting SVG using `GdkPixbuf.PixbufLoader.new_with_type('svg')`, close the loader, obtain its pixbuf, and create a `Gdk.Texture` with `new_for_pixbuf`.
5. Set the texture with `Gtk.Image.set_from_paintable()` and keep a stable logical `pixel_size`.
6. Log decode failures and display the fallback icon.

Choose foregrounds from the application's theme or explicit design palette, covering light, dark, and both high-contrast appearances. Do not impose one application's hardcoded colors on another. A manually colored texture does not inherit per-widget suggested/destructive foregrounds. Use the native path when that behavior matters, or explicitly implement and verify the required states.

For lifecycle handling, render immediately, subscribe to appearance changes while the image is realized, rerender on realization, and disconnect on unrealization. Reconnect when realized again. Use `notify::dark` and `notify::high-contrast` for appearance changes. Store handler IDs and avoid duplicates. A style manager outlives individual images, so undisconnected closures can retain removed widgets.

Use `Adw.StyleManager.get_default()` for the normal application-wide case; use the display-specific manager if the host application has display-specific appearance settings. Always recolor from the original SVG, either cached or reread, so a previous replacement does not prevent later theme changes.

Literal replacement is appropriate only for a controlled asset token. For arbitrary imported SVGs, use an XML-aware transformation of the intended fills/strokes; do not broadly replace every color or edit SVG structure with regexes. Keep fixed-color logos on their established regular-image path.

If a rasterized icon is blurry at high scaling, inspect the decoded pixel dimensions. `pixel_size` controls layout, not source raster resolution. Decode for logical size times display scale and refresh on scale changes, or keep the vector-backed native icon path.

## Package and Resolve Assets

Use the project's existing asset directory and build system. Make runtime loading agree with the installed layout:

- For GResources, add the SVG to the manifest, compile the bundle, and ensure it is registered before icon lookup. Resource aliases determine runtime paths independently of source filenames.
- For loose files, include the assets in install/package rules and resolve them through a configured data directory or a stable module-relative path. Never depend on the process working directory.
- In GJS, a filesystem module's `import.meta.url` can anchor relative asset lookup. Modules loaded from `resource://` do not have a filesystem path; use resource APIs for those assets.
- If the project ships both loose files and a resource bundle, keep both populated where their consumers require them. A manifest entry alone does not satisfy a loader that reads loose files.
- Verify source, staged install, and sandboxed package layouts where supported. Custom in-app icons do not require replacing the desktop application's launcher icon.

## Verify the Actual Artwork

Run the host project's existing focused checks and add coverage proportional to the change. Static checks can validate SVG palette conventions and package contents, but cannot establish rendered colors or correct theme-signal cleanup.

When modifying rendering or packaging, verify:

- The custom icon appears at the intended size, with no clipping or fallback substitution.
- Existing visible widgets update when switching light/dark appearance; high contrast works in both appearances. Verify fixed accents remain intact.
- Relevant normal, disabled, hover, and suggested/destructive states have legible contrast. Check high display scaling when rasterizing.
- Removing and recreating the view does not leave duplicate appearance handlers or stale icons.
- A missing or malformed asset produces the intended fallback and useful error reporting.
- The build/resource manifest includes the asset, and the packaged layout matches the runtime loader. Use a temporary staging prefix or inspect build outputs when install testing is needed.

Do not alter the user's installed app just to test icons unless installation was requested. Report whether checks were static or included a live GTK render.

## API References

- [GTK 4 migration: icon resources and symbolic rendering](https://docs.gtk.org/gtk4/migrating-3to4.html)
- [Gtk.IconTheme.add_resource_path](https://docs.gtk.org/gtk4/method.IconTheme.add_resource_path.html)
- [Gtk.IconTheme.add_search_path](https://docs.gtk.org/gtk4/method.IconTheme.add_search_path.html)
- [GTK symbolic SVG format](https://github.com/GNOME/gtk/blob/main/docs/symbolic-icons.md)
- [Adw.StyleManager](https://gnome.pages.gitlab.gnome.org/libadwaita/doc/1-latest/class.StyleManager.html)

GNOME Shell extensions use `St.Icon` and St CSS such as `-st-icon-style`; do not copy those APIs into a GTK application.
