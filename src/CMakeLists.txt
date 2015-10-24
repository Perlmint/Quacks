set(QUACKS_MAIN_UIS mainwindow.ui)
append_prefix(QUACKS_MAIN_UIS)
qt5_wrap_ui(QUACKS_MAIN_UI_HDRS ${QUACKS_MAIN_UIS})
set(QUACKS_MAIN_MOCS mainwindow.h)
append_prefix(QUACKS_MAIN_MOCS)
qt5_wrap_cpp(QUACKS_MAIN_MOC ${QUACKS_MAIN_MOCS})

set(QUACKS_MAIN_SRCS
  mainwindow.h
  mainwindow.cpp
  ${QUACKS_MAIN_UI_HDRS}
  ${QUACKS_MAIN_MOC}
  )

if(NOT ANDROID AND NOT IOS)
  list(APPEND QUACKS_MAIN_SRCS main.cpp)
endif()

append_prefix(QUACKS_MAIN_SRCS)
