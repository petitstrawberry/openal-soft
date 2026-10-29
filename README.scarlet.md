# Native Scarlet

The `scarlet-1.19` branch ports the C implementation in OpenAL Soft 1.19.1 to
the native Scarlet AArch64 ABI. This version does not require the C++ exception
and unwind runtime used by newer OpenAL Soft releases.

Use the matching Scarlet SDK with pthread synchronization and process-private
semaphores. Build a position independent static archive and package the public
AL/ALC ABI in `libopenal.so`. Build the native code-generation tools for the
host first; with CMake 4, configure that sub-build with
`CMAKE_POLICY_VERSION_MINIMUM=3.5`. Disable host audio backends, examples,
utilities, Wave output and embedded HRTF data for the current profile.

The port includes the scheduler declaration explicitly, omits an unused
pthread-exit helper, uses native debug traps for optional error trapping, and
uses the AArch64 AdvSIMD compilation baseline instead of Linux procfs for NEON
selection. Ambisonic scale tables are declared `extern` for modern compilers.
Other platforms retain their existing thread-exit, signal and CPU detection
paths.

Acceptance creates a loopback context, mixes a real PCM source to stereo,
checks nonzero output and native error reporting, and releases all resources
from native LWJGL. The current profile has no Scarlet hardware playback or
capture backend; loopback acceptance does not establish audible playback.

Retain the upstream LGPL notices and source when distributing this library.
