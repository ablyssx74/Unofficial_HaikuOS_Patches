### How to apply realtime audio in Haiku OS 


1. Make sure have a supported audio card and it supports custom flags.<br> ( explained below step 3 )<br>

2. Download libmedia_realtime_patchx86_64.zip, unzip and copy/drag libmedia.so to drop_libmedia.so_here symlink folder.<br>

3. Find your audio card settings file.  If you don't know or don't have one you can search the Haiku source https://grok.nikisoft.one/opengrok/xref/haiku/src/add-ons/kernel/drivers/audio<br>
There you will see the supported audio devices and if they have a custom settings file for your audio card. <br>

4. Once you've found the custom file you will need to copy it to ~/config/settings/kernel/drivers/ Then edit it to apply the realtime audio.  Then restart the media server.<br><br>

5. For example, here is my hda custom file.  I have edited for 192khz 1024 buffers.
```
# Settings file for the HDA driver
#
#  This file should be moved to the directory
#  ~/config/settings/kernel/drivers/
#  Uncomment and edit any of the setting lines you wish to set.
#  The values shown are just the defaults.
#  Don't uncomment any that you don't need to differ from the default.

#  Remember that latency will probably be at least
#   2*buffer_frames/sample_rate
#  For real time audio, a buffer of 1024 frames at 192000
#  should be good (<15ms).
#  You will need more buffers if they're small.
#  e.g.:
#     play_buffer_frames	1024
#     play_buffer_count	4

###########################################################

play_buffer_frames	1024
play_buffer_count	4

record_buffer_frames	1024
record_buffer_count	4
```

## How to build Haiku Source libmedia
To build  libmedia 
1. git clone https://review.haiku-os.org/haiku
2. git clone https://review.haiku-os.org/buildtools if needed. See online documentation for when it's required.
3. cd haiku
4. ./configure --target-arch x86_64 ( apply your --target-arch if different )
5. jam -q libmedia.so
6. cp generated/objects/haiku/x86_64/release/kits/media/libmedia.so /boot/system/non-packaged/lib/
7. Restart Media Server
