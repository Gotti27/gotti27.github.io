---
layout: page
title: 3D laser scanner
description: an implementation of a line laser scanner
# img: assets/img/avogador-logo.png
importance: 2
category: work
related_publications: false
---

Article todo, [github repo](https://github.com/Gotti27/3d-scanner) [project statement](https://github.com/Gotti27/3d-scanner/blob/master/2023_Geometric_and_3D_Computer_Vision_Final_Project.pdf)

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3d-scanner/setting.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3d-scanner/pointcloud.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3d-scanner/mesh.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<!--

Once the laser plane equation and the camera pose are estimated, the laser edge on the object identifies the intersection between camera rays and the laser plane and it is possible to retrieve the 3D coordinates of those points wrt the plate reference system. The plate rotation makes possible to complete the object scan.
-->