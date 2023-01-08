# render PNG assets

gtk3_asset_names = run_command(
  'cat', 'assets' / 'assets.txt',
  check : true
).stdout().split()

subdir('assets')

# compile CSS

sass_path = meson.current_source_dir() / 'sass'
sass_depend_files = run_command(
  'find',
  sass_path,
  '-name', '_*.scss',
  check : true
).stdout().split()

gtk3_stylesheets = []

foreach mode : get_option('modes')
  foreach contrast : get_option('contrasts')
    foreach accent : get_option('accents')
      variant = mode + '-' + contrast + '-' + accent

      input_scss = configure_file(
        input: meson.current_source_dir() / 'sass' / 'gtk.scss.in',
        output : 'gtk-' + variant + '.scss',
        configuration : {
          'mode' : mode,
          'contrast' : contrast,
          'accent' : accent,
          'sass_path' : meson.current_source_dir() / 'sass',
          }
        )

      output_css = 'gtk-main' + variant + '.css'

      gtk3_stylesheets += custom_target(
        output_css,
        input : input_scss,
        output : output_css,
        command : [sassc, sassc_options, '@INPUT@', '@OUTPUT@'],
        build_by_default : true,
        depend_files : sass_depend_files
        )

      # generate the gresource XML

      gresource_xml_array = [
        '<?xml version="1.0" encoding="UTF-8"?>',
        '<gresources>',
        '<gresource prefix="/org/gtk/gruvbox-theme">'
        ]

      foreach asset : gtk3_asset_names
        gresource_xml_array += [
          '<file>assets/' + asset + '.png</file>',
          '<file>assets/' + asset + '@2.png</file>'
          ]
      endforeach

      gresource_xml_array += ['<file>' + output_css + '</file>']

      gresource_xml_array += [
        '</gresource>',
        '</gresources>'
        ]

      # compile the gresource

      gresource_xml = configure_file(
        capture : true,
        command : ['echo', gresource_xml_array],
        output : 'gtk-' + variant + '.gresource.xml'
        )

      #TODO use gnome.compile_resources()???
      gresource = custom_target(
        'gresource-' + variant,
        input : gresource_xml,
        output : 'gtk-' + variant + '.gresource',
        command : [
          glib_compile_resources,
          '--sourcedir=@OUTDIR@',
          '--sourcedir=@CURRENT_SOURCE_DIR@',
          '--target=@OUTPUT@',
          '@INPUT@'
          ],
        depends : [gtk3_assets, gtk3_hidpi_assets, gtk3_stylesheets],
        depend_files : [gtk3_asset_files, gtk3_hidpi_asset_files],
        build_by_default : true
        )

      # install gresource
      meson.add_install_script(
        install_file,
        gresource.full_path(),
        '$MESON_INSTALL_DESTDIR_PREFIX' / install_dir / common_dirs.get('gtk3') / 'gtk.gresource'
        )

      # install gtk.css files

      gtk3_css = configure_file(
        capture : true,
        command : ['echo', '@import url("resource:///org/gtk/gruvbox-theme/' + output_css + '");'],
        output : 'gtk-' + variant + '.css'
        )

      install_data(
        gtk3_css,
        install_dir : prefix / install_dir / common_dirs.get('gtk3'),
        rename : ['gtk.css']
        )

      # install thumbnail

      install_data(
        variant != 'dark' ? 'thumbnail/light.png' : 'thumbnail/dark.png',
        install_dir : prefix / install_dir / common_dirs.get('gtk3'),
        rename : ['thumbnail.png']
        )
    endforeach
  endforeach
endforeach
