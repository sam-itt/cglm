project('cglm', 'c',
    version : '0.7.9', 
    license : 'mit',
    default_options : [
        'c_std=c11',
        'werror=true',
        'warning_level=2',
        'buildtype=release'
    ]
)

cc = meson.get_compiler('c')

cglm_deps = cc.find_library('m', required : false)

cglm_args = []
build_args = []

if get_option('default_library') == 'static'
    cglm_args = '-DCGLM_STATIC'
endif

if host_machine.system() == 'windows'
    build_args = '-DCGLM_EXPORTS'
endif

cglm_inc = include_directories('include')

cglm_src = files(
    'src/affine.c',
    'src/affine2d.c',
    'src/bezier.c',
    'src/box.c',
    'src/cam.c',
    'src/curve.c',
    'src/ease.c',
    'src/euler.c',
    'src/frustum.c',
    'src/io.c',
    'src/mat2.c',
    'src/mat3.c',
    'src/mat4.c',
    'src/plane.c',
    'src/project.c',
    'src/quat.c',
    'src/ray.c',
    'src/sphere.c',
    'src/vec2.c',
    'src/vec3.c',
    'src/vec4.c'
)

install_subdir('include/cglm', install_dir : get_option('includedir'))

cglm_lib = library('cglm',
    cglm_src,
    install : true,
    dependencies : cglm_deps,
    c_args : [ build_args, cglm_args ]
)

cglm_dep = declare_dependency(
    link_with : cglm_lib,
    dependencies : cglm_deps,
    compile_args : cglm_args,
    include_directories : cglm_inc,
    version : meson.project_version()
)

if meson.version().version_compare('>= 0.54.0')
    meson.override_dependency('cglm', cglm_dep)
endif


pkg = import('pkgconfig')

pkg.generate(
    name : 'cglm',
    libraries : cglm_lib,
    extra_cflags : cglm_args,
    version : meson.project_version(),
    url : 'https://github.com/recp/cglm',
    description : 'OpenGL Mathematics (glm) for C'
)

if get_option('build_tests') == true

test_src = files(
    'test/runner.c',
    'test/src/test_euler.c',
    'test/src/test_bezier.c',
    'test/src/test_cam.c',
    'test/src/test_struct.c',
    'test/src/test_clamp.c',
    'test/src/test_common.c',
    'test/src/tests.c'
)

test_exe = executable('tests', 
    test_src, 
    dependencies : cglm_dep,
    c_args : '-DGLM_TESTS_NO_COLORFUL_OUTPUT'
)

test('cglm.tests', test_exe)

endif
