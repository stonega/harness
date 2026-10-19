# GNOME Recommendations

Use this baseline for a standalone native GNOME app or `apps/gnome`. For a Shell
extension alone, apply the tooling, extension, and verification guidance relevant
to that surface; the GTK application scaffold is optional.

These recommendations are extracted from `~/Gnome/wallshader`. Its GJS interface,
development scripts, Meson installation, and isolated tests are the example. Adapt
the architecture to the new product rather than copying wallpaper features.

## Stack and dependencies

- Application runtime: GJS with JavaScript ES modules.
- Native interface: GTK 4 and libadwaita.
- Platform integration: Gio and GLib for files, actions, settings, and D-Bus.
- Development tooling: Bun for package management, build scripts, and logic tests;
  Biome for JavaScript and JSON linting and formatting.
- Build and installation: Meson and Ninja when installing a desktop application.

Install GNOME libraries and their introspection data through the distribution's
packages. Document the package names and minimum GJS, GTK, and libadwaita versions
required by the APIs used. GI API versions such as `Gtk?version=4.0` and
`Adw?version=1` identify the API family, not the minimum library release.

Keep application UI in native widgets. Add WebKitGTK or other runtime dependencies
only when the product needs their capabilities. Bun is a development dependency;
the installed application should launch with GJS without Bun or a source checkout.

## Bootstrap and layout

Initialize tooling with Bun and add Biome as a development dependency:

```sh
bun init -y
bun add -d @biomejs/biome
```

Use `"type": "module"` in `package.json`. Adapt this layout alongside the shared
documentation directories from `SKILL.md`:

```text
src/
  main.js              # Application lifecycle and actions
  window.js            # Native window and interface
  paths.js             # Application ID and asset/XDG paths
  model.js             # Runtime-independent state and validation
  storage.js           # Gio-backed persistence
data/
  <app-id>.desktop
  <command>.in         # Installed launcher template
  icons/
  style.css            # Only when custom GTK styling is needed
scripts/
  run.sh
  install.sh
  test-native.sh
tests/
  model.test.js
  native-smoke.js
meson.build
```

Choose a project-specific reverse-DNS application ID. Use it consistently in
`Adw.Application`, the desktop filename, and the application icon name. Keep script
names and package commands aligned with the files actually generated:

```json
{
  "scripts": {
    "start": "./scripts/run.sh",
    "check": "biome check .",
    "format": "biome check --write .",
    "test": "bun test",
    "test:native": "./scripts/test-native.sh"
  }
}
```

Add a build command only when assets need compilation or bundling. GJS source
changes normally require restarting the application. Exclude generated assets,
native build directories, and test artifacts from Git and linting. Keep GTK CSS
outside Biome's web CSS parser; verify it by loading it in GTK.

## Application and interface

- Use [GJS ES module imports](https://gjs.guide/guides/gjs/intro.html), including
  `gi://Gtk?version=4.0`, `gi://Adw?version=1`, `gi://Gio`, and `gi://GLib`.
  Keep relative module imports explicit with `.js` extensions.
- Create an `Adw.Application`; register application actions and accelerators during
  startup. On activation, present the existing window or create an
  `Adw.ApplicationWindow`. Use `GObject.registerClass` for GObject subclasses.
- Keep model validation and state transitions independent of `gi://` imports so
  Bun can test them. Keep native file operations and desktop effects in adapters.
- Follow the [GNOME HIG](https://developer.gnome.org/hig/) and the applicable
  preferences in [DESIGN.md](DESIGN.md). Let Adwaita supply fonts, controls, accent
  colors, and system light/dark behavior. Keep custom CSS small and semantic.
- Use [adaptive libadwaita layouts](https://gnome.pages.gitlab.gnome.org/libadwaita/doc/1-latest/adaptive-layouts.html)
  where needed: toolbars and header bars for window chrome, and split views with
  breakpoints for navigation or inspectors. Choose widgets available in the
  documented minimum libadwaita version and keep narrow windows usable.
- Prefer native file dialogs, actions, menus, and symbolic icons. Give icon buttons
  accessible names and tooltips; retain keyboard navigation and visible focus.
- Use GLib's XDG directory helpers for configuration, data, and cache paths.
  Validate loaded state and use atomic replacement for JSON persistence. Use
  GSettings when schema-backed preferences fit the product.
- Keep slow I/O off the GTK main loop and release signal handlers, file monitors,
  timers, and cancellable work when their owner closes.

## Launching and installation

- Make `scripts/run.sh` executable. Resolve its project directory independently
  of the caller's working directory, then launch `gjs -m` with the entrypoint and
  forwarded arguments. Resolve assets from module or installed paths.
- Use Meson to install GJS modules and private assets under the app's data
  directory, and generate a launcher from the configured install prefix. Keep the
  launcher usable after moving away from the checkout.
- Install the desktop file under `share/applications` and the app icon under
  `share/icons/hicolor`. Its `Exec` must name the installed launcher and its `Icon`
  must match the installed icon. Keep toolbar icons in the app's private icon path.
- Use [Meson's GNOME module](https://mesonbuild.com/Gnome-module.html) for relevant
  desktop/icon cache updates and resource or schema compilation when needed.
- Provide a repeatable user installation script that handles both first setup and
  Meson reconfiguration. Wallshader uses `~/.local` as its default prefix:

  ```sh
  meson setup build-native --prefix="$HOME/.local"
  meson compile -C build-native
  meson install -C build-native
  ```

- For packages, stage installation with `DESTDIR` and validate the launcher,
  desktop file, icons, and runtime dependencies from that tree. Preserve bundled
  dependencies' licenses and notices. Keep release versions aligned across app
  metadata, Meson, and package definitions that exist in the new project.

## Verification

- Run `bun run check` and `bun test` for the generated model and scripts. Keep the
  Bun suite free of imports that require GJS.
- Run a native smoke test with actual GJS/GTK widgets for window startup, important
  interactions, persistence, and native integrations. Use a separate test
  application ID, temporary XDG config/data/cache directories, and
  `GSETTINGS_BACKEND=memory` so tests leave the user's preferences untouched.
- Isolate D-Bus when testing calls that control desktop services. Stub external
  controllers in application smoke tests; exercise real integration in a private
  session. Desktop changes belong to explicit product actions, never startup or
  tests against the user's session.
- Check wide and narrow layouts, light and dark appearances, keyboard/focus
  behavior, and relevant error states. Store useful screenshots and diagnostics
  in an ignored artifacts directory. Report graphical checks that could not run.
- Verify the installed launcher from outside the checkout. Validate the desktop
  file with `desktop-file-validate` when available.

## Optional integrations

### Embedded rendering

Wallshader uses WebKitGTK 6.0 only for a locally bundled WebGL canvas; controls and
dialogs remain GTK widgets. Use this pattern only for a rendering requirement
that warrants WebKit. Bundle assets with Bun, keep the installed renderer local,
validate messages at the GJS boundary, and surface initialization failures. Test
the rendering path on the target runtime; keep driver workarounds specific to a
demonstrated problem.

### GNOME Shell extensions

- Add an extension only when the feature needs compositor or Shell integration.
  Keep it separate from the GTK app, with its own metadata and declared Shell
  versions. Verify APIs against those versions using the
  [GJS extension guide](https://gjs.guide/extensions/).
- Keep heavyweight rendering and application work in a separate process. Use an
  explicit, validated D-Bus protocol for app/extension coordination. When managing
  windows spawned through `Meta.WaylandClient`, identify ownership with
  `owns_window` rather than window titles.
- Track resources created during enable and release signals, timers, D-Bus
  exports, owned processes, and actors during disable or failure.
- Test integration in a separate headless/nested Shell with a private D-Bus,
  runtime directory, XDG directories, virtual displays, and isolated settings.
  Exercise enable/disable cleanup and relevant monitor changes there.

### Panel menu popup alignment

- Center a panel menu popup horizontally beneath the panel button that opened it:
  the popup's horizontal midpoint should match the button's horizontal midpoint
  whenever space allows. Use the full clickable button, including any icon and
  status text, as the anchor.
- Use GNOME Shell's popup positioning facilities instead of fixed pixel offsets.
  Keep the popup within the current monitor's usable bounds, allowing edge
  clamping when exact centering would put it offscreen.
- Verify alignment with the actual panel button contents, including changing
  status text, and with the button near a monitor edge.

## Wallshader example sources

When `~/Gnome/wallshader` is available, useful example sources include:

- `AGENTS.md`, `docs/design/architecture.md`, and `docs/implementation/setup.md` for
  runtime boundaries, development workflow, and verification rationale.
- `src/main.js`, `src/storage.js`, and `scripts/run.sh` for application lifecycle,
  persistence, and source launch patterns.
- `meson.build`, `data/wallshader.in`, and `scripts/install.sh` for installation.
- `scripts/test-native.sh` and `scripts/test-shell.sh` for test isolation.

The shader dependency, wallpaper behavior, GNOME 50 target, rendering workarounds,
and existing release destinations are Wallshader-specific choices. Select these
independently for each new project.
