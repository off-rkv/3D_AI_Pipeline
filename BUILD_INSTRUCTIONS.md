# QuadriFlow Build Instructions for Windows

## Prerequisites
- Visual Studio 2026 (or compatible version)
- CMake 3.1 or higher
- Eigen 3.4.0 (installed at `C:/libs/eigen-3.4.0`)
- Boost (Anaconda installation will be detected automatically)

## Quick Build Steps

### 1. Clone the Repository
```cmd
cd C:\Users\offrk\Desktop
git clone https://github.com/off-rkv/3D_AI_Pipeline.git
cd 3D_AI_Pipeline
```

### 2. Build QuadriFlow
Open **x64 Native Tools Command Prompt for VS 2026** and run:

```cmd
cd C:\Users\offrk\Desktop\3D_AI_Pipeline\QuadriFlow
mkdir build
cd build

cmake .. -DCMAKE_BUILD_TYPE=Release -DEIGEN_INCLUDE_DIR="C:/libs/eigen-3.4.0"
cmake --build . --config Release
```

### 3. Run QuadriFlow
```cmd
cd Release
quadriflow.exe --help
```

## What Was Fixed

The original QuadriFlow repository had a **runtime library mismatch** on Windows MSVC:
- The lemon library was compiled with `/MD` (dynamic runtime)
- The QuadriFlow executable was compiled with `/MT` (static runtime)

This caused linking errors:
```
error LNK2038: mismatch detected for 'RuntimeLibrary': value 'MD_DynamicRelease' doesn't match value 'MT_StaticRelease'
error LNK1169: one or more multiply defined symbols found
```

**Solution Applied:**
- Modified `QuadriFlow/CMakeLists.txt` to force `/MD` runtime for all components
- Ensured consistent runtime library linkage across the entire project
- Protected MSVC default compiler flags from being overwritten

## Troubleshooting

### If build fails with "Could not find Eigen"
Make sure Eigen is installed at `C:/libs/eigen-3.4.0`. Adjust the path in the cmake command if needed:
```cmd
cmake .. -DCMAKE_BUILD_TYPE=Release -DEIGEN_INCLUDE_DIR="YOUR_EIGEN_PATH"
```

### If build fails with "Could not find Boost"
The build will automatically detect Boost from Anaconda at:
`C:/Users/offrk/anaconda3/Library/lib/cmake/Boost-1.82.0/`

If you have Boost elsewhere, set `BOOST_ROOT`:
```cmd
set BOOST_ROOT=C:\path\to\boost
cmake .. -DCMAKE_BUILD_TYPE=Release -DEIGEN_INCLUDE_DIR="C:/libs/eigen-3.4.0"
```

### Clean Build
If you need to start fresh:
```cmd
cd C:\Users\offrk\Desktop\3D_AI_Pipeline\QuadriFlow
rmdir /S /Q build
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DEIGEN_INCLUDE_DIR="C:/libs/eigen-3.4.0"
cmake --build . --config Release
```

## Branch Information

The fixes are on branch: `claude/fix-quadriflow-linking-01KpMtUhTQ256mAES5U7QJor`

To switch to this branch:
```cmd
git checkout claude/fix-quadriflow-linking-01KpMtUhTQ256mAES5U7QJor
```

## Test Environment

Tested successfully with:
- Visual Studio 2026 (MSVC 19.50.35717.0)
- Windows SDK 10.0.26100.0
- Eigen 3.4.0
- Boost 1.82.0
- CMake 3.1+
