foreach mode : get_option('modes')
  foreach contrast : get_option('contrasts')
    foreach accent : get_option('accents')
      variant = mode + '-' + contrast + '-' + accent

      index_theme = configure_file(
        input: 'index.theme.in',
        output : 'index.theme.' + variant,
        configuration : {
          'variant_name' : variant,
          }
        )

      install_dir = datadir / 'themes' / variant

      install_data(
        index_theme,
        rename: 'index.theme',
        install_dir : prefix / install_dir,
        )
    endforeach
  endforeach
endforeach

common_dirs = {
  'gtk3' : 'gtk-3.0',
  'gtk4' : 'gtk-4.0'
}

foreach theme : get_option('themes')
  subdir(common_dirs.get(theme))
endforeach
