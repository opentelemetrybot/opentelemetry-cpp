# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

add_library(
  opentelemetry_exporter_prometheus
  src/exporter.cc src/exporter_options.cc src/exporter_factory.cc
  src/collector.cc src/exporter_utils.cc)

set_target_properties(opentelemetry_exporter_prometheus
                      PROPERTIES EXPORT_NAME prometheus_exporter)
set_target_version(opentelemetry_exporter_prometheus)

target_include_directories(
  opentelemetry_exporter_prometheus
  PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>"
         "$<INSTALL_INTERFACE:include>")

target_link_libraries(
  opentelemetry_exporter_prometheus
  PUBLIC opentelemetry_metrics prometheus-cpp::core
  PRIVATE prometheus-cpp::pull)

otel_add_component(
  COMPONENT
  exporters_prometheus
  TARGETS
  opentelemetry_exporter_prometheus
  FILES_DIRECTORY
  "include/opentelemetry/exporters/prometheus"
  FILES_DESTINATION
  "include/opentelemetry/exporters"
  FILES_MATCHING
  PATTERN
  "*.h")

if(BUILD_TESTING)
  add_subdirectory(test)
endif()
