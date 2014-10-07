#
# Copyright (c) 2014 CNRS
# Authors: Florent Lamiraux
#
#
# This file is part of hpp_tutorial
# hpp_tutorial is free software: you can redistribute it
# and/or modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation, either version
# 3 of the License, or (at your option) any later version.
#
# hpp_tutorial is distributed in the hope that it will be
# useful, but WITHOUT ANY WARRANTY; without even the implied warranty
# of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# hpp_tutorial  If not, see
# <http://www.gnu.org/licenses/>.

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

SET(CXX_DISABLE_WERROR TRUE)
INCLUDE(cmake/base.cmake)
INCLUDE(cmake/python.cmake)

SET(PROJECT_NAME hpp-r-stepps)
SET(PROJECT_DESCRIPTION
  "Robot models and files for project R-STEPPS."
)
SET(PROJECT_URL "")

FINDPYTHON()

SETUP_PROJECT()

# Catkin stuff
FIND_PACKAGE(catkin REQUIRED COMPONENTS
  hpp_ros)
CATKIN_PACKAGE()

# Activate hpp-util logging if requested
SET (HPP_DEBUG FALSE CACHE BOOL "trigger hpp-util debug output")
IF (HPP_DEBUG)
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DHPP_DEBUG")
ENDIF()

ADD_DOC_DEPENDENCY("hpp-core >= 3")
ADD_REQUIRED_DEPENDENCY("hpp-corbaserver >= 3")
ADD_REQUIRED_DEPENDENCY("hpp_ros")

# Create and install executable running the corba server
ADD_EXECUTABLE (hpp-tutorial-server
  src/tutorial.cc
)
# Link executable with hpp-corbaserver library
PKG_CONFIG_USE_DEPENDENCY (hpp-tutorial-server hpp-corbaserver)
# Install executable
INSTALL (TARGETS hpp-tutorial-server DESTINATION ${CMAKE_INSTALL_BINDIR})


SET(CATKIN_PACKAGE_SHARE_DESTINATION
  ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME})

install(FILES launch/init.launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  )
install(FILES urdf/rstepps.urdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf
  )
install(FILES srdf/rstepps.srdf
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/srdf
  )
install(FILES rviz/config.rviz
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/rviz
  )
install (FILES
  src/hpp/corbaserver/r_stepps/robot.py
  src/hpp/corbaserver/r_stepps/__init__.py
  DESTINATION ${PYTHON_SITELIB}/hpp/corbaserver/r_stepps)

SETUP_PROJECT_FINALIZE()
