## https://ffmpeg.org/pipermail/ffmpeg-devel/2020-June/263746.html

This patch supports hardware-accelerated decode in ffmpeg on Jetson platforms.

ffmpeg application can use accelerated decode to read video files in the following elementary/container formats and dump them in YUV 420 format:
H.264, H.265, VP8, VP9. MPEG2, MPEG4 
(Note: The ffmpeg package does not support MPEG4 container files).

Steps to apply the patch:
1) Clone the ffmpeg master branch:
git clone git://source.ffmpeg.org/ffmpeg
2) cd ffmpeg
3) Apply the patch 0001-ffmpeg-Accelerated-hardware-decode-support-on-Jetson.patch
Command to apply the patch:
git apply 0001-ffmpeg-Accelerated-hardware-decode-support-on-Jetson.patch

Steps to compile:
1) Install "jetson_multimedia_api" package from latest Jetpack release.
Follow link to install jetpack on jetson: https://developer.nvidia.com/embedded/jetpack
2) Install below depended packages on target using the command (if not already installed by jetpack):
sudo apt install libv4l-dev
sudo apt install gcc
sudo apt install pkg-config
sudo apt install libegl1-mesa-dev
3) Run following commands to build and install:
./configure --enable-nvv4l2dec --enable-libv4l2 --enable-shared --extra-libs="-L/usr/lib/aarch64-linux-gnu/tegra -lnvbuf_utils" --extra-cflags="-I /usr/src/jetson_multimedia_api/include/"
make
sudo make install
export LD_LIBRARY_PATH=/usr/local/lib/
4) Run following commands to decode:
H264:  ffmpeg -c:v h264_nvv4l2dec -i <input file> <output file>
H265:  ffmpeg -c:v hevc_nvv4l2dec -i <input file> <output file>
MPEG2: ffmpeg -c:v mpeg2_nvv4l2dec -i <input file> <output file>
MPEG4: ffmpeg -c:v mpeg4_nvv4l2dec -i <input file> <output file>
VP8:   ffmpeg -c:v vp8_nvv4l2dec -i <input file> <output file>
VP9:   ffmpeg -c:v vp9_nvv4l2dec -i <input file> <output file>

Example command: 
ffmpeg -c:v mpeg2_nvv4l2dec -i libmpeg2bug.ts output_420.yuv
(Input file is referenced from https://samples.ffmpeg.org/MPEG2/)
