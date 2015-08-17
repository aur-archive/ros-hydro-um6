
pkgdesc="ROS - The um6 package provides a C++ implementation of the CH Robotics serial protocol, and a corresponding ROS node for publishing standard ROS orientation topics from a UM6."
url='http://www.ros.org/'

pkgname='ros-hydro-um6'
pkgver='0.0.2'
_pkgver_patch=0
arch=('i686' 'x86_64')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-hydro-roscpp
  ros-hydro-serial
  ros-hydro-message-generation
  ros-hydro-catkin
  ros-hydro-sensor-msgs)
makedepends=('cmake' 'git' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-hydro-roscpp
  ros-hydro-serial
  ros-hydro-sensor-msgs
  ros-hydro-message-runtime)
depends=(${ros_depends[@]})

_tag=release/hydro/um6/${pkgver}-${_pkgver_patch}
_dir=um6
source=("${_dir}"::"git+https://github.com/clearpath-gbp/um6-release.git"#tag=${_tag})
md5sums=('SKIP')

build() {
  # Use ROS environment variables
  /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/hydro/setup.bash ] && source /opt/ros/hydro/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/hydro \
        -DPYTHON_EXECUTABLE=/usr/bin/python2 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
