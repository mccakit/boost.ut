### Documentation

https://boost-ext.github.io/ut/

### API Reference

https://mccakit.github.io/boost_ut/

### Packaging and COnsumption

Currently I only support clang, and module consumption is only based on bmi's, just pkgconfig package works, here is my conanfile recipe I use:

```
from conan import ConanFile
import os
import subprocess


class boost_ut(ConanFile):

    name = "boost.ut"
    version = "main"
    settings = "os", "arch", "compiler", "build_type"

    def source(self):
        subprocess.run(
            f'bash -c "git clone --recurse-submodules --shallow-submodules --depth 1 git@github.com:mccakit/boost_ut.git -b {self.version}"',
            shell=True,
            check=True,
        )

    def build(self):
        cmake_toolchain = self.conf.get("user.mccakit:cmake", None)
        os.chdir("boost_ut")
        pkgconf_path = ":".join(
            os.path.join(dep.package_folder, "lib", "pkgconfig")
            for dep in self.dependencies.values()
        )
        os.environ["PKG_CONFIG_LIBDIR"] = pkgconf_path
        cmake_prefix_path = ";".join(
            dep.package_folder for dep in self.dependencies.values()
        )
        subprocess.run(
            f'bash -c "cmake -B build -G Ninja -DCMAKE_PREFIX_PATH=\\"{cmake_prefix_path}\\" -DCMAKE_TOOLCHAIN_FILE={cmake_toolchain} -DCMAKE_INSTALL_PREFIX={self.package_folder} -DBOOST_UT_DISABLE_MODULE=OFF -DBOOST_UT_BUILD_TESTS=OFF -DBOOST_UT_BUILD_EXAMPLES=OFF"',
            shell=True,
            check=True,
        )
        subprocess.run(
            f'bash -c "cmake --build build --parallel"', shell=True, check=True
        )
        subprocess.run(f'bash -c "cmake --install build"', shell=True, check=True)

```
Here is a snippet for consuming it in cmake:

```
find_package(PkgConfig REQUIRED)
pkg_check_modules(BOOST.UT REQUIRED IMPORTED_TARGET boost.ut)
...
target_link_libraries(app PRIVATE PkgConfig::BOOST.UT)
```


