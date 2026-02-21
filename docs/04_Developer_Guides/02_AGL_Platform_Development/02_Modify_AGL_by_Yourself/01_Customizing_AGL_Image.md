---
title: Overview
---

## Customizing agl-image-weston with AGL

This guide provides step-by-step instructions for customizing the AGL `agl-image-weston` image using Yocto. We cover two common customization tasks: adding packages like glmark2 and resizing the image.

### Prerequisites

- AGL build environment set up and initialized following [01_Getting_Started/02_Building_AGL_Image](../../../01_Getting_Started/02_Building_AGL_Image/)
- Your build directory is 'build-agl' that setups by aglsetup.sh
- Yocto knowledge and familiarity with bitbake and layer concepts

---

### 1. Adding glmark2 to core-image-weston

glmark2 is a benchmarking tool available in `meta-openembedded/meta-oe/recipes-benchmark/glmark2`. There are multiple ways to add it to your image.

#### Method 1: Using local.conf (Quickest)

The simplest approach is to add glmark2 directly to your `local.conf` file.

1. Navigate to your build directory:
   ```bash
   cd build-agl
   ```

2. Open the `conf/local.conf` file in your preferred editor:
   ```bash
   vim conf/local.conf
   ```

3. Add the following line at the end of the file:
   ```conf
   IMAGE_INSTALL:append = " glmark2"
   ```

4. Save and close the file.

5. Build the image:
   ```bash
   bitbake agl-image-weston
   ```

The glmark2 package will now be included in your image.

#### Method 2: Creating a Custom Image Recipe

For more control and reusability, create a custom image recipe.

1. Create a custom layer in your AGL workspace (if not already present):
   ```bash
   bitbake-layers create-layer meta-custom-agl
   ```

2. Create a new image recipe in the custom layer:
   ```bash
   mkdir -p meta-custom-agl/recipes-core/images
   touch meta-custom-agl/recipes-core/images/agl-image-weston.bbappend
   ```

3. Edit the new recipe file and add the following content:
   ```bb
   IMAGE_INSTALL:append = " glmark2"
   ```

4. Add your custom layer to the build configuration:
   ```bash
   bitbake-layers add-layer meta-custom-agl
   ```

5. Build your custom image:
   ```bash
   bitbake agl-image-weston
   ```

---

### 2. Resizing the Image by 2 GB

By default, `agl-image-weston` has a predefined size. To increase the image size by 2 GB, you need to adjust the `IMAGE_ROOTFS_SIZE` variable.

#### Calculating the Size Increase

- 1 GB = 1024 MB = 1,048,576 KB
- 2 GB = 2048 MB = 2,097,152 KB

#### Method 1: Via local.conf

1. Open your `conf/local.conf` file:
   ```bash
   vim conf/local.conf
   ```

2. Find or add the `IMAGE_ROOTFS_SIZE` variable. If it doesn't exist, add it:
   ```conf
   IMAGE_ROOTFS_SIZE = "409600"
   ```
   
   This example sets the base size to 400 MB. Adjust the value as needed for your use case.

3. To increase by 2 GB, modify the value:
   ```conf
   # Original: 400 MB (409600 KB)
   # Increase by 2 GB (2097152 KB)
   # New value: 2400 MB (2457600 KB) or 2.4 GB
   IMAGE_ROOTFS_SIZE = "2457600"
   ```

4. Save the file and rebuild:
   ```bash
   bitbake -c cleansstate agl-image-weston
   bitbake agl-image-weston
   ```

#### Method 2: Via Custom Image Recipe

Prerequire: You already created agl-image-weston.bbappend followed as 'Method 2: Creating a Custom Image Recipe' steps.

1. Add to your agl-image-weston.bbappend recipe:
   ```bb
   IMAGE_ROOTFS_SIZE = "2457600"
   ```

2. Rebuild the image:
   ```bash
   bitbake agl-image-weston
   ```

---

### Additional Resources

- [Yocto Mega Manual - IMAGE_ROOTFS_SIZE](https://docs.yoctoproject.org/ref-manual/variables.html#var-IMAGE_ROOTFS_SIZE)
