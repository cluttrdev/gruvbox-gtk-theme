mode = get_option('mode')
contrast = get_option('contrast')
accent = get_option('accent')

variant = mode + '-' + contrast + '-' + accent

theme_name = 'gruvbox-' + variant

index_theme = configure_file(
  input: 'index.theme.in',
  output : 'index.theme.' + variant,
  configuration : {
    'theme_name' : theme_name,
    'icon_theme_name' : 'gruvbox-' + mode + '-' + accent
    }
  )

install_dir = datadir / 'themes' / theme_name

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
