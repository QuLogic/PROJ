project('proj', 'c', version : '4.9.3', meson_version : '0.43.0')

cc = meson.get_compiler('c')
conf = configuration_data()

LIBM = cc.find_library('m', required : false)

# Check for C99 math functions
math_test_code = '''
#include <math.h>
int main(void) {
    int q;
    return (int)(hypot(3.0, 4.0) + atanh(0.8) + cbrt(8.0) +
                 remquo(100.0, 90.0, &q) +
                 remainder(100.0, 90.0) + copysign(1.0, -0.0));
}
'''
conf.set10('HAVE_C99_MATH',
    cc.links(math_test_code, name : 'C99 math functions'))

conf.set('HAVE_LOCALECONV', cc.has_function('localeconv'))

# ---------------------------------------------------------------------------
# Check for JNI support.
# ---------------------------------------------------------------------------

JNI_INCLUDE = []
JNI_ENABLED = false

if get_option('jni')
    add_languages('java')
    jni_dir = get_option('jni-dir')
    if jni_dir != ''
        foreach dir : jni_dir.split(':')
            JNI_INCLUDE += ['-I' + dir]
        endforeach
        if cc.has_header('jni.h', args : JNI_INCLUDE)
            JNI_ENABLED = true
        else
            error('Could not find @0@/jni.h'.format(jni_dir))
        endif
    else
        if cc.has_header('jni.h')
            JNI_ENABLED = true
        else
            error('Could not find jni.h')
        endif
    endif
endif
conf.set('JNI_ENABLED', JNI_ENABLED)
JNI = declare_dependency(compile_args : JNI_INCLUDE)

# ---------------------------------------------------------------------------
# Provide a mechanism to disable real mutex support (if lacking win32 or
# posix mutexes for instance).
# ---------------------------------------------------------------------------

THREADS = []
if get_option('mutex')
    MUTEX_SETTING = 'pthread'
    THREADS = dependency('threads')
    conf.set('HAVE_PTHREAD_MUTEX_RECURSIVE',
        cc.has_header_symbol('pthread.h', 'PTHREAD_MUTEX_RECURSIVE'))
else
    MUTEX_SETTING = 'stub'
endif

pkgdatadir = join_paths(get_option('datadir'), meson.project_name())
subdir('src')
subdir('man')
subdir('nad')
if JNI_ENABLED
    subdir('jniwrap')
endif

pkg = import('pkgconfig')
pkg.generate(
    name : meson.project_name(),
    filebase : 'proj',
    version : meson.project_version(),
    description : 'Cartographic Projections Library',
    libraries : libproj)
