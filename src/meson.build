mode = get_option('mode')
contrast = get_option('contrast')
accent = get_option('accent')

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

common_dirs = {
  'gtk3' : 'gtk-3.0',
  'gtk4' : 'gtk-4.0'
}

foreach theme : get_option('themes')
  subdir(common_dirs.get(theme))
endforeach
