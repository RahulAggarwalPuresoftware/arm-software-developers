---
processors : ["Neoverse-N1"]
tools : ["gcc", "g++"]
software : ["linux"]
title: "Find Non-Portable Code"
linkTitle: "Find Non-Portable Code"
type: docs
weight: 4
hide_summary: true
description: >
    Use aarch64 Porting Advisor to find non-portable source code
---

A tool which may be useful is [aarch64 Porting Advisor](https://github.com/arm-hpc/porting-advisor). It is a quick way to identify architecture specific code. Porting Advisor is not needed for the simple example presented above, but if there are architecture specific intrinsics hiding deep in a larger project it can help find them. 

To use Porting Advisor install it using the commands below.

```console
git clone https://github.com/arm-hpc/porting-advisor.git
cd porting-advisor
sudo python3 setup.py install
```

Run Porting Advisor by supplying the directory with the source code to be analyzed. For example, to try it on the open source KasmVNC project use the commands below.

```console
git clone https://github.com/kasmtech/KasmVNC.git
porting-advisor KasmVNC 
```

The output is 

```console
| Elapsed Time: 0:00:00                                                                                                              
413 files scanned.
KasmVNC/common/rfb/scale_sse2.cxx: 55 other issues
KasmVNC/common/rfb/scale_sse2.cxx:74 (SSE2_halve): architecture-specific intrinsic: _mm_loadu_si128
KasmVNC/common/rfb/scale_sse2.cxx:75 (SSE2_halve): architecture-specific intrinsic: _mm_loadu_si128
KasmVNC/common/rfb/scale_sse2.cxx:62 (SSE2_halve): architecture-specific intrinsic: _mm_set_epi32
KasmVNC/common/rfb/scale_sse2.cxx:63 (SSE2_halve): architecture-specific intrinsic: _mm_set_epi32
KasmVNC/common/rfb/scale_sse2.cxx:64 (SSE2_halve): architecture-specific intrinsic: _mm_set_epi32
KasmVNC/common/rfb/scale_sse2.cxx:61 (SSE2_halve): architecture-specific intrinsic: _mm_setzero_si128
KasmVNC/common/rfb/scale_sse2.cxx:78 (SSE2_halve): architecture-specific intrinsic: _mm_unpackhi_epi8
KasmVNC/common/rfb/scale_sse2.cxx:80 (SSE2_halve): architecture-specific intrinsic: _mm_unpackhi_epi8
KasmVNC/common/rfb/scale_sse2.cxx:77 (SSE2_halve): architecture-specific intrinsic: _mm_unpacklo_epi8
KasmVNC/common/rfb/scale_sse2.cxx:79 (SSE2_halve): architecture-specific intrinsic: _mm_unpacklo_epi8

Use --output FILENAME.html to generate an HTML report.
```

Porting Advisor scans the directory and immediately points out architecture specific extensions. Check out the usage instructions for more information.

[<-- Return to Learning Path](/cloud/intrinsics/#sections)

