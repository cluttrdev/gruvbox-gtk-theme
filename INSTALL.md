# Installing from source

## Getting the source

To get the source, either clone the git repository with e.g.

```bash
git clone https://github.com/cluttrdev/gruvbox-gtk-theme --depth 1
```

Or, download and extract a [snapshot](https://github.com/cluttrdev/gruvbox-gtk-theme/archive/main.zip) of the main git branch, or the latest [release tarball](https://github.com/cluttrdev/gruvbox-gtk-theme/releases/latest).

## Dependencies

#### Build dependencies

The following packages are required for building the theme:

 * `meson` build system (version 0.53.0 or later)
 * `sassc` for compiling stylesheets from [Sass](https://sass-lang.com/) files
 * `glib-compile-resources` for compiling `GResource` files
 * `inkscape` for rendering asset files

## Building and Installation

This project uses the [Meson](https://mesonbuild.com/) build system, refer to its documentation for further information about the build process.

The following instructions should work for most common cases.

#### Setup and configure a build directory

First you need to setup and configure a new build directory (e.g. `build/`) from the cloned/extracted source code directory.

You should at least configure the build prefix with `--prefix=` option, usually `/usr` for system wide installation, or `$HOME/.local` for installing for your user only.
Additionally you may set any theme specific [build options](#build-options) according to your needs and preferences, with `-Doption=value` command line argument.

For example, configure to install in your home directory, and to build the `dark-medium-yellow` variant with:

```bash
meson setup --prefix=$HOME/.local -Dmode=dark -Dcontrast=medium -Daccent=yellow build/
```

The build options can later be changed with `meson configure` command, e.g.

```bash
meson configure --prefix=/usr build/
```

#### Build and install

Build and install the theme according to your configuration by running the following:

    meson install -C build/

## Build options

Theme specific build options can be set or changed with `meson configure -Doption=value <build_directory>` e.g.

```bash
meson configure -Dthemes=gtk3 -Dmode=light -Dcontrast=medium -Daccent=blue build/
```

Option     | Choices                                          | Default value | Description
------     | -------                                          | ------------- | -----------
`themes`   | `gtk3, gtk4`                                     | `gtk3,gtk4`   | List of themes to build
`mode`     | `dark, light`                                    | `dark`        | The brightness mode to build 
`contrast` | `hard, medium, soft`                             | `medium`      | The contrast configuration to build
`accent`   | `red, green, yellow, blue, purple, aqua, orange` | `yellow`      | The accent color variant to build

## Uninstallation

Manually remove the theme directories from your install location, e.g.

```bash
rm -rf $HOME/.local/share/themes/gruvbox-dark-medium-yellow/
```

