# How to Build a Qt Application Using Visual Studio Code

# Introduction

`Qt` is a popular cross-platform framework for building GUI applications. While Qt Creator is the official IDE for Qt development, many developers prefer using **Visual Studio Code** for its flexibility and extensive extensions. In this guide, we will walk through the process of setting up and building a Qt application using **VS Code**.

### Video Tutorial :-
[![Watch the video](https://img.youtube.com/vi/j2Gfkrajb1M/0.jpg)](https://www.youtube.com/watch?v=j2Gfkrajb1M)

## Prerequisites

Before we start, ensure you have the following installed on your system:

* Qt 6.5 or later

* Visual Studio Code

* Qt Extension Pack for VS Code

* CMake

* A C++ Compiler (such as MinGW or MSVC)

### Step 1: Create a New Project Folder
Start by creating a new empty folder for your project. In this example, we'll name it `LoginForm`.

### Step 2: Open VS Code and Add Files
Open VS Code and create the following files inside the LoginForm folder:

- main.cpp
- mainwindow.cpp
- mainwindow.h
- mainwindow.ui

### Step 3: Install the Qt Extension Pack

To work with Qt inside VS Code, you need to install the Qt Extension Pack. Open the Extensions panel in VS Code, search for **Qt Extension Pack**, and install it.

Once installed, you must register your Qt installation:

1. Open **Command Palette** (Ctrl + Shift + P).
2. Type **Qt** and select **Register Qt Installation**.
3. Choose the folder where you installed Qt.

### Step 4: Add a CMakeLists.txt File
To build the project, we need a CMakeLists.txt file. Below is a basic structure:
```CMake
cmake_minimum_required(VERSION 3.16)
project(projectname LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

find_package(Qt6 6.5 REQUIRED COMPONENTS Core Widgets)

qt_standard_project_setup()

add_executable(projectname
    WIN32 MACOSX_BUNDLE
    main.cpp mainwindow.cpp mainwindow.h)

target_link_libraries(projectname
    PRIVATE
        Qt::Core
        Qt::Widgets
)

include(GNUInstallDirs)

install(TARGETS projectname
    BUNDLE  DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

qt_generate_deploy_app_script(
    TARGET LoginForm
    OUTPUT_SCRIPT deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})
```
Save this file in the project root folder.

### Step 5: Configure the CMake Build System
To generate the build files:
1. Open Command Palette (Ctrl + Shift + P).
2. Type CMake: Select a Kit and select the Qt toolchain.
3. If build files are not generated automatically, delete the build folder and try again.

### Step 6: Design the UI
Open `mainwindow.ui` in Qt Designer and add two buttons: `OK` and `Cancel`. Change their object names to **btnOk** and **btnCancel**, respectively.

### Step 7: Implement the Main Application Logic

In `main.cpp`, set up a simple Qt application:
```c++
#include "mainwindow.h"

#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}
```
In `mainwindow.h`, declare a slot function for button clicks:
```C++
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>

QT_BEGIN_NAMESPACE
namespace Ui {
class MainWindow;
}
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private slots:
    void on_btnOk_Clicked();
    void on_btnCancel_Clicked();

private:
    Ui::MainWindow *ui;
};
#endif // MAINWINDOW_H
```

In `mainwindow.cpp`, implement the slot function:

```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    connect(ui->btnOk, SIGNAL(clicked()),this,SLOT(on_btnOk_Clicked()));
    connect(ui->btnCancel, SIGNAL(clicked()),this,SLOT(on_btnCancel_Clicked()));
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::on_btnOk_Clicked()
{
    qDebug() << "Ok button clicked";
}

void MainWindow::on_btnCancel_Clicked()
{
    qDebug() << "Cancel button clicked";
}

```
### Step 8: Build and Run the Project

1. Open Command Palette (Ctrl + Shift + P).
2. Type CMake: Build and run the build process.
3. Once built successfully, run the application. The window should appear.

### Step 9: Debugging and Running
To debug, create a launch.json configuration:

1. Open Run and Debug (Ctrl + Shift + D).
2. Click Create a launch.json file.
3. Select C++ (Windows).
4. Choose Visual Studio Debugger.
5. Save and start debugging.
