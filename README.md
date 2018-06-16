# lumbar_sagittal_seg
This tutorial explains how to segment spinal cords on images with sagittal orientation and thick slices. In general we highly recommend to segment the cord on isotropic resolution data, or data acquired axially. However, in some cases, you might want to obtain a rough delineation of the cord from sagittal scans. Here's a suggestion on how to do it.

## Dependencies

SCT development version (master:51f9f79ffad0cbdb3b865273faa4aa605fced2df). Next stable release: v3.2.1.


## Getting started

Below is an example of a sagittal scan. You notice that the spinal cord is difficult to delineate on the axial view, due to the low resolution in the right-left direction:

<img src="https://github.com/sct-pipeline/lumbar_sagittal_seg/blob/master/imgs/3d_view_original.png" width="600">

The strategy here is to resample the scan to an isotropic resolution, and then segment the cord using manual initialization.

```bash
# resample
sct_resample -i T2.nii.gz -mm 1x1x1 -o T2r.nii.gz
# segment with manual initialization
sct_propseg -i T2r.nii.gz -c t2 -init-centerline viewer
```

The image below shows how to do the manual initialization. Here, we only label until the lower tip of the spinal cord, which stops at around T12-L1. Below the spinal cord is the cauda equine.
<img src="https://github.com/sct-pipeline/lumbar_sagittal_seg/blob/master/imgs/manual_labeling.png" width="600">

And here is the resulting cord segmentation:

<img src="https://github.com/sct-pipeline/lumbar_sagittal_seg/blob/master/imgs/seg_overlaid.gif" width="600">

If you wish to resample the segmentation at the same resolution of the native image, you can do so using the following command:

```bash
# resample back to T2 native space
sct_register_multimodal -i T2r_seg.nii.gz -d T2.nii.gz -identity 1 -x linear
```

Note, here we used linear interpolation in order to keep the partial volume information, but you can also output a binary segmentation by replacing `-x linear` by `-x nn`. Here is the output:

<img src="https://github.com/sct-pipeline/lumbar_sagittal_seg/blob/master/imgs/seg_overlaid_resampled.gif" width="600">


## License

The MIT License (MIT)

Copyright (c) 2018 École Polytechnique, Université de Montréal

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

