# Copyright 1996-2021 Cyberbotics Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Webots Makefile system
#
# You may add some variable definitions hereafter to customize the build process
# See documentation in $(WEBOTS_HOME_PATH)/resources/Makefile.include


CXX_SOURCES = \
  core/MainApplication.cpp \
  core/StandardPaths.cpp \
  devices/Device.cpp \
  devices/Motor.cpp \
  graph2d/Graph2D.cpp \
  gui/GenericWindow.cpp \
  gui/MainWindow.cpp \
  motion_editor/AddStateDialog.cpp \
  motion_editor/ListWidget.cpp \
  motion_editor/Motion.cpp \
  motion_editor/MotionEditor.cpp \
  motion_editor/MotionGlobalSettings.cpp \
  motion_editor/MotionPlayer.cpp \
  motion_editor/MotionWidget.cpp \
  motion_editor/Pose.cpp \
  motion_editor/PoseEditor.cpp \
  motion_editor/PoseWidget.cpp \
  motion_editor/MotorTargetState.cpp \
  motion_editor/MotorTargetStateWidget.cpp \
  widgets/AccelerometerWidget.cpp \
  widgets/CameraWidget.cpp \
  widgets/CategoryWidget.cpp \
  widgets/CompassWidget.cpp \
  widgets/DeviceWidget.cpp \
  widgets/DeviceWidgetFactory.cpp \
  widgets/DistanceSensorWidget.cpp \
  widgets/GPSWidget.cpp \
  widgets/GyroWidget.cpp \
  widgets/InertialUnitWidget.cpp \
  widgets/LidarWidget.cpp \
  widgets/LightSensorWidget.cpp \
  widgets/MotorWidget.cpp \
  widgets/PositionSensorWidget.cpp \
  widgets/RadarWidget.cpp \
  widgets/RangeFinderWidget.cpp \
  widgets/ScalarSensorWidget.cpp \
  widgets/SensorWidget.cpp \
  widgets/TouchSensorScalarWidget.cpp \
  widgets/TouchSensorVectorialWidget.cpp \
  widgets/VectorialSensorWidget.cpp

HPP_FILES_TO_MOC = \
  core/MainApplication.hpp \
  gui/GenericWindow.hpp \
  motion_editor/AddStateDialog.hpp \
  motion_editor/ListWidget.hpp \
  motion_editor/Motion.hpp \
  motion_editor/MotionEditor.hpp \
  motion_editor/MotionPlayer.hpp \
  motion_editor/MotionWidget.hpp \
  motion_editor/Pose.hpp \
  motion_editor/PoseEditor.hpp \
  motion_editor/PoseWidget.hpp \
  motion_editor/MotorTargetState.hpp \
  motion_editor/MotorTargetStateWidget.hpp \
  widgets/AccelerometerWidget.hpp \
  widgets/CameraWidget.hpp \
  widgets/CompassWidget.hpp \
  widgets/DistanceSensorWidget.hpp \
  widgets/GPSWidget.hpp \
  widgets/GyroWidget.hpp \
  widgets/InertialUnitWidget.hpp \
  widgets/LidarWidget.hpp \
  widgets/LightSensorWidget.hpp \
  widgets/MotorWidget.hpp \
  widgets/PositionSensorWidget.hpp \
  widgets/RadarWidget.hpp \
  widgets/RangeFinderWidget.hpp \
  widgets/SensorWidget.hpp \
  widgets/TouchSensorScalarWidget.hpp \
  widgets/TouchSensorVectorialWidget.hpp \
  widgets/VectorialSensorWidget.hpp

USE_C_API = true

null :=
space := $(null) $(null)
WEBOTS_HOME_PATH=$(subst $(space),\ ,$(strip $(subst \,/,$(WEBOTS_HOME))))
include $(WEBOTS_HOME_PATH)/resources/Makefile.include

# Qt linking
CFLAGS += -std=c++11
MOC_SOURCES = $(addprefix $(BUILD_DIR)/,$(notdir $(HPP_FILES_TO_MOC:.hpp=.moc.cpp)))
MOC_DIRECTORIES = $(sort $(dir $(HPP_FILES_TO_MOC)))
OBJECTS += $(addprefix $(BUILD_GOAL_DIR)/,$(notdir $(MOC_SOURCES:.cpp=.o)))

ifeq ($(OSTYPE),windows)
 CFLAGS += -D_USE_MATH_DEFINES
 QT_INCLUDE_DIR = $(WEBOTS_HOME)/include/qt
 MOC = moc
 MOC_PLATFORM_FLAGS = -D_WIN32
 DYNAMIC_LIBRARIES += -L"$(WEBOTS_HOME)/msys64/mingw64/bin" -lQt5Core -lQt5Gui -lQt5Widgets
 INCLUDE += -isystem "$(QT_INCLUDE_DIR)/QtCore" -isystem "$(QT_INCLUDE_DIR)/QtGui" -isystem "$(QT_INCLUDE_DIR)/QtWidgets"
endif

ifeq ($(OSTYPE),linux)
 QT_INCLUDE_DIR = $(WEBOTS_HOME)/include/qt
 QT_LIB_DIR = $(WEBOTS_HOME)/lib/webots
 MOC = "$(WEBOTS_HOME)/bin/qt/moc"
 MOC_PLATFORM_FLAGS = -D__linux__
 DYNAMIC_LIBRARIES += -L"$(QT_LIB_DIR)" -lQt5Core -lQt5Gui -lQt5Widgets
 INCLUDE += -isystem "$(QT_INCLUDE_DIR)/QtCore" -isystem "$(QT_INCLUDE_DIR)/QtGui" -isystem "$(QT_INCLUDE_DIR)/QtWidgets"
endif

ifeq ($(OSTYPE),darwin)
 WBCFLAGS += -Wno-unknown-pragmas
 FRAMEWORKS_DIR = $(WEBOTS_HOME)/Contents/Frameworks
 INCLUDE += -F"$(FRAMEWORKS_DIR)"
 FRAMEWORKS += -F"$(FRAMEWORKS_DIR)"
 DYNAMIC_LIBRARIES += -bind_at_load $(FRAMEWORKS)
 MOC = "$(WEBOTS_HOME)/bin/qt/moc"
 MOC_PLATFORM_FLAGS = -D__APPLE__
 FRAMEWORKS += -framework QtCore -framework QtGui -framework QtWidgets
endif

vpath %.cpp $(BUILD_DIR)
vpath %.hpp $(MOC_DIRECTORIES)

.PRECIOUS: $(MOC_SOURCES)


$(BUILD_DIR)/%.moc.cpp: %.hpp
	@echo "# moc-generating" $(notdir $<)
	$(SILENT)$(MOC) $(MOC_PLATFORM_FLAGS) $(INCLUDE:-isystem:-I) $< -o $@

$(BUILD_GOAL_DIR)/$(MAIN_TARGET): $(OBJECTS)
