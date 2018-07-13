---
layout: default
title: FAQ
section: support/faq
---
<style>
  h3 {
    margin: 40px 0px 20px 0px;
  }
</style>
<div class="page-header">
  <h1>FAQ Section</h1>
</div>

### Frequently Asked Questions

#### Why does Gazebo crash in my VM?

When attempting to launch gazebo I get the following error:

	VMware: vmw_ioctl_command error Invalid argument.
	
A workaround for this problem is to force GZ to use OpenGL2 by exporting the following environment variable:

	$ export SVGA_VGPU10=0
	
See also [this bug report](https://bugs.launchpad.net/stellarium/+bug/1577494) for a deeper explanation of the issue and the original fix.


