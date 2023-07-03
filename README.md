# Intel RealSense Modular Component

## Getting Started
For Linux Distros, the simplest way of getting the camera server is by using Docker. The command

```
git clone https://github.com/viamrobotics/viam-camera-realsense/
cd viam-camera-realsense/
make appimage
```

will build a Docker image and compile a binary for the `aarch64` architecture, which will be placed in the `bin/` directory.

## Building From Source

### Dependencies

- [librealsense](https://github.com/IntelRealSense/librealsense)
  - `git checkout` and install from source. 
  - be sure to use cmake flags `cmake .. -DBUILD_EXAMPLES=false -DBUILD_GRAPHICAL_EXAMPLES=false -DCMAKE_BUILD_TYPE=Release`
- [libjpegturbo](https://github.com/libjpeg-turbo/libjpeg-turbo)
- [libprotobuf](https://github.com/protocolbuffers/protobuf)
- [Viam C++ SDK](https://github.com/viamrobotics/viam-cpp-sdk/)
  - specifically `libviamsdk`, `libviamapi`, and `libviam_rust_utils`

then do `make camera-module` to compile the binary.

## Attributes and Sample Config

The attributes for the module are as follows:
- `sensors` (required): a list that contain the strings `color` and/or `depth`. The sensor that comes first in the list is designated the "main sensor" and will be the image that gets returned by `get_image` calls and what will appear in the Control tab on app.viam.
- `width_px`, `height_px`: the width and height of the output images. If the RealSense cannot produce the requested resolution, the component will fail to be built.
- `little_endian_depth`: a bool that specifices whether raw depth data should be encoded in a little-endian byte order. By default it is `false`, and encodes the raw depth data in a big-endian byte order.
- `enable_point_clouds`: a bool that specifies whether the `get_pointcloud` method of the camera should be enabled. By default it is `false`, as enabling the method consumes large amounts of CPU and slows down the operation of the camera.
```
{
  "components": [
    {
      "name": "myRealSense",
      "attributes": {
        "sensors": ["color","depth"],
        "width_px": 640,
        "height_px": 480,
        "little_endian_depth": false,
        "enable_point_clouds": false
      },
      "namespace": "rdk",
      "type": "camera",
      "model": "viam:camera:realsense"
    }
  ],
  "modules": [
    {
      "executable_path": "/home/user/viam-camera-realsense",
      "name": "intel"
    }
  ],
}
```
