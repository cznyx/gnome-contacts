project('gnome-contacts', [ 'vala', 'c' ],
  version: '3.27.90',
  license: 'GPL2+',
  meson_version: '>= 0.37',
)

gnome = import('gnome')
i18n = import('i18n')

# Add our custom VAPI dir
add_project_arguments(
  ['--vapidir', join_paths(meson.source_root(), 'vapi')],
  language: 'vala'
)

# Some variables
config_h_dir = include_directories('.')
contacts_prefix = get_option('prefix')
datadir = join_paths(contacts_prefix, get_option('datadir'))
bindir = join_paths(contacts_prefix, get_option('bindir'))
libdir = join_paths(contacts_prefix, get_option('libdir'))
libexecdir = join_paths(contacts_prefix, get_option('libexecdir'))
locale_dir = join_paths(contacts_prefix, get_option('localedir'))
pkgdatadir = join_paths(datadir, meson.project_name())
pkglibdir = join_paths(libdir, meson.project_name())
po_dir = join_paths(meson.source_root(), 'po')

# Dependencies
min_eds_version = '3.13.90'
min_folks_version = '0.11.4'
min_glib_version = '2.44' # We also use this value for --target-glib, so scratch the micro version.

folks = dependency('folks', version: '>=' + min_folks_version)
folks_eds = dependency('folks-eds', version: '>=' + min_folks_version)
gee = dependency('gee-0.8')
gio_unix = dependency('gio-unix-2.0', version: '>=' + min_glib_version)
glib = dependency('glib-2.0', version: '>=' + min_glib_version)
gmodule_export = dependency('gmodule-export-2.0', version: '>=' + min_glib_version)
gnome_desktop = dependency('gnome-desktop-3.0')
goa = dependency('goa-1.0')
gtk = dependency('gtk+-3.0', version: '>= 3.22.0')
libebook = dependency('libebook-1.2', version: '>=' + min_eds_version)
libedataserver = dependency('libedataserver-1.2', version: '>=' + min_eds_version)
libedataserverui = dependency('libedataserverui-1.2', version: '>=' + min_eds_version)

# Libraries
math = meson.get_compiler('c').find_library('m')

# Options
cheese_enabled = (get_option('with-cheese') != 'no')
if cheese_enabled
  cheese_required = (cheese_enabled == 'yes')
  cheese = dependency('cheese', required: cheese_required)
  cheese_gtk = dependency('cheese-gtk', version: '>= 3.3.91', required: cheese_required)
  cheese_enabled = cheese.found() and cheese_gtk.found()
endif

telepathy_enabled = get_option('telepathy')
if telepathy_enabled
  folks_telepathy = dependency('folks-telepathy', version: '>=' + min_folks_version)
  telepathy_glib = dependency('telepathy-glib', version: '>= 0.22.0')
endif

manpage_enabled = get_option('with-manpage')

# Configuration
conf = configuration_data()
conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())
conf.set_quoted('G_LOG_DOMAIN', meson.project_name())
conf.set_quoted('LOCALEDIR', locale_dir)
conf.set_quoted('PACKAGE_NAME', meson.project_name())
conf.set_quoted('PACKAGE_STRING', meson.project_name())
conf.set_quoted('PACKAGE_VERSION', meson.project_version())
conf.set_quoted('PKGDATADIR', pkgdatadir)
conf.set_quoted('PKGLIBDIR', pkglibdir)
conf.set_quoted('VERSION', meson.project_version())
conf.set('GOA_API_IS_SUBJECT_TO_CHANGE', true)
configure_file(output: 'config.h', configuration: conf)

# Post-install scripts
meson.add_install_script('meson_post_install.py')

# Subfolders
subdir('data')
subdir('po')
subdir('src')
subdir('man')
