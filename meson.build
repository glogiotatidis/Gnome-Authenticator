# See https://github.com/mesonbuild/meson/issues/226
project('org.gnome.Authenticator', 'c',
	version: '0.1.1')


python = find_program('python3')
git = find_program('git', required : false)
intltool_merge = find_program('intltool-merge')
if git.found()  
    GITVERSION = run_command(git, 'rev-parse', 'HEAD').stdout().strip()
else
    GITVERSION = ''
endif

message('Looking for dependencies')
py3 = find_program('python3')
glib = dependency('glib-2.0')
gobject = dependency('gobject-2.0')
gir = dependency('gobject-introspection-1.0')
gtk = dependency('gtk+-3.0', version : '>=3.16')

message('Getting python install dir')
r = run_command(py3, '-c', 'from distutils.sysconfig import get_python_lib; print(get_python_lib())')
if r.returncode() != 0
    error('Cannot find python install dir')
endif
python_dir = r.stdout().strip()
message('Python install dir found : YES ' + python_dir)

# Configuration params
conf = configuration_data()
conf.set('PACKAGE_URL', 'https://github.com/bil-elmoussaoui/Gnome-Authenticator')
conf.set('LIBDIR', get_option('prefix') + '/' + get_option('libdir'))
conf.set('DATADIR', get_option('prefix') + '/' + get_option('datadir') + '/Authenticator/')
conf.set('LOCALE_DIR', get_option('prefix') + '/' + get_option('datadir') + '/locale/')
conf.set('PACKAGE_NAME', 'Gnome Authenticator')
conf.set('PACKAGE', 'org.gnome.Authenticator')
conf.set('GITVERSION', GITVERSION)
conf.set('VERSION', meson.project_version())
conf.set('BUILDDIR', meson.current_build_dir())
conf.set('PYTHONDIR', python_dir)

subdir('data')
subdir('po')


run_command(python, '-m', 'compileall', meson.current_source_dir() + '/Authenticator')
install_subdir('Authenticator', install_dir: python_dir)

message('Preparing init file')
configure_file(input : 'gnome-authenticator.in', output : 'gnome-authenticator', configuration : conf)

configure_file = '@0@/gnome-authenticator'.format(meson.current_build_dir())
install_data(configure_file, install_dir: 'bin')
meson.add_install_script('meson_post_install.sh')
