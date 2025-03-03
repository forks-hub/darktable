We're proud to announce the new feature release of darktable, 4.4.0!

The github release is here: [https://github.com/darktable-org/darktable/releases/tag/release-4.4.0](https://github.com/darktable-org/darktable/releases/tag/release-4.4.0).

As always, please don't use the autogenerated tarball provided by
github, but only our tar.xz file. The checksums are:

```
$ sha256sum darktable-4.4.0.tar.xz
??? darktable-4.4.0.tar.xz
$ sha256sum darktable-4.4.0.dmg
??? darktable-4.4.0.dmg
$ sha256sum darktable-4.4.0.exe
??? darktable-4.4.0.exe
```

When updating from the stable 4.2.x series, please bear in
mind that your edits will be preserved during this process, but the new
library and configuration will no longer be usable with 4.2.x.

You are strongly advised to take a backup first.

#### Important note: to make sure that darktable can keep on supporting the raw file format for your camera, *please* read [this post](https://discuss.pixls.us/t/raw-samples-wanted/5420?u=lebedevri) on how/what raw samples you can contribute to ensure that we have the *full* raw sample set for your camera under CC0 license!

Since darktable 4.2:

- Almost ??? commits to darktable+rawspeed
- ??? pull requests handled
- ?? issues closed

## The Big Ones

The following is a summary of the main features added to darktable
4.4. Most of these features are described more fully in the user manual.

- Allows for multiple presets to be defined and applied automatically
  on matching images. Each preset after the first one will create a
  new module instance just on top of the current module (so it applies
  after).

  To better view which module instance corresponds to which preset the
  module label is set to the preset label. This module label will be
  changed if some parameters on the module are changed. The module's
  label will be cleared if no preset match or will be set to the new
  preset label otherwise. If the module label has been hand edited it
  will be kept as-is and will never be updated automatically.

  A new option named "automatically update module name" (activated by
  default) has been introduced to allow to fully disable the module's
  label auto update.

- Rework the module default parameters and make them usable
  in copy/paste, presets and styles.

  Many modules have default parameters based on image metadata or
  current workflow:

  - Exposure: use a specific default in scene referred workflow,
  - Denoise profile: is camera and ISO specific,
  - Lens correction: is camera and lens specific,
  - Base curve: is camera sensor specific,
  - White balance: is metadata specific,
  - Orientation: is metadata specific,
  - Color calibration: is metadata specific.

  For all those modules it is now possible to paste parameters and
  ensure that proper default metadata based will be used. This is
  achieved by selecting the "Reset" corresponding column in the
  preset and style dialog.

  For presets a new option can be selected in the dialog for the
  preset to be using the module's default parameters on which it is
  applied. The option is named "Reset all module parameters to their
  default values"

  This new generic feature has permitted to clean-up some hacks to try
  to do the same kind support at the module level and which was
  anyway limited.

- Rework the workflow setting, we now have the choice between:

  - Scene-refrerred (Filmic)
  - Scene-referred (Sigmoid)
  - Display-referred (Legacy)
  - None

  The chromatic adaptation preference has been removed and is implied
  by the workflow. The Color calibration module is used when in
  Scene-referred and While balance otherwise.

  Finally, using None workflow and the two new features above
  (multiple presets support and reset to default parameters metadata
  based) one can create any other kind of workflow. For example it is
  possible to use Sigmoid with the White balance module.

- Add support for Color Harmony Guide lines in RYB vectorscope.

  There are 9 color harmony guides proposed:
  - Monochromatic
  - Analogous
  - Analogous complementary
  - Complementary
  - Split complementary
  - Dyad
  - Triad
  - Tetrad
  - Square

  Those guides can be used to shift colors of the key areas of a picture
  to match one of the color harmonies. One can see the color harmony guides
  a bit like the composition guides but related to colors and not to
  composition.

- Various code cleanups and improved performances. All module's SSE2
  code path have been removed (as the optimized parallel code
  generated by the compiler is faster) or code optimized in the
  following modules. This leads to a speed gain of 5% to 25%:

  - Dithering
  - Graduated Density
  - Input Color Profile
  - Color Look up Table
  - Borders
  - Surface Blur
  - Vignette
  - Retouch
  - Denoise Profile
  - Invert
  - Local Contrast with Local Laplacian
  - Low-pass Filter
  - RGB Levels
  - Input Color Profile
  - Lowlight vision
  - Velvia
  - Split-toning
  - Nega Doctor
  - Channel Mixer RGB (CIECAM16, linear Bradford, XYZ and nonlinear Bradford).
  - Filmic (legacy)
  - Filmic RGB (including highlights reconstruction)
  - Color Balance
  - Color Balance (legacy)
  - Levels (legacy)
  - Relight
  - Liquify
  - Color Mapping
  - High-pass filter
  - Shadows and Highlights
  - Lens
  - Grain
  - Monochrome
  - In-paint opposed highlights reconstruction

  - The interpolation algorithms (Bicubic, Bilinear, Lanczos2,
    Lanczos3) used by modules doing warp or scaling of pixels. The old
    Crop and Rotate and the new Perspective Correction modules are now
    running faster.

  - The gaussian generator used by many modules: Censorize, Denoise
    Profile, Lowpass, Diffuse & Sharpen, Defringe, RAW Chromatic
    Aberrations, Base Curve, Perspective Correction, Filmic RGB,
    Retouch, Tone Equalizer and Zone System (deprecated). Meaning all
    those modules have parts now running faster.

  - The box blur filter used by the focus peaking, the guided filter
    for blending, the new highlight recovery algorithms, and the
    Bloom, High-pass, Haze removal, and Soften modules. Meaning all
    those modules and features have parts now running faster.

  - The Edge-Avoiding a-trous Wavelet used by those modules Contrast
    Equalizer and Denoise (Profiled). Meaning those modules have parts
    now running faster.

  - Some part of the bilateral filter have been improved for better
    performances. This is used in multiple modules like Monochrome,
    Lowpass Filter, Shadow and Highlights, Censorize, Retouch, Color
    Mapping, Rotation and Perspective and Local Contrast. Meaning all
    those modules have parts now running faster.

  - All the blending modes in Lab & RGB for the Display & Scene
    referred workflows have been optimized.

  - The color adaptation matrices have been transposed to allow for
    vectorization.

  - The luminance mask calculation for the Tone Equalizer.

  - Loader for JPEG2000 file format.

  - The "acquire clusters" operation in the Color Mapping module has
    been speed up by a factor of 30 to 200, making the results
    perceptually instantaneous on clicking the button.

- Add global <kbd>right-click</kbd> and drag to fix image
  rotation. This can now be used at any moment in darkroom as long as
  the currently focused module is not using this shortcut. This allows
  for fast rotation correction without having to open the Rotation and
  Perspective module.

## Other Changes

- Overhaul the color picker code. No longer unnecessarily run pickers
  in many cases, resulting in speedups. The picker code is now tuned
  for contemporary processors. It uses recent OpenMP features,
  resulting in more succinct code. The pickers now only runs a
  time-consuming denoise pass when used via the filmic module (in
  which case removing noise makes the automatic tuning more
  robust). No longer warn when working on monochrome images. Various
  other cleanup, de-duplication, optimization, and generally tidying.

- Overhaul of pixelpipe code and it's caching strategy with
  significant performance gains while developing in darkroom.

- Modernize the histogram calculation code. Remove SSE code (which
  provides no speed-ups), but use it as a model for the optimized code
  using recent OpenMP features. Remove various unused bits of code,
  and provide a consistent internal API. In certain cases this code
  will produce marginally more accurate results. In some cases the new
  code uses substantially less memory.

- Add OpenCL support to the Sigmoid module.

- Add OpenMP support to XCF export for better performances.

- Add OpenMP support to the RGBE loader for better performances.

- Do not invalidate snapshot anymore when the history is changed
  (compressed or reset). All snapshot are now stored with their full
  history and can be reconstructed properly.

- Module "Levels" has been deprecated, use "Levels RGB" instead.

- Module "Contrast Brightness Saturation" has been deprecated, use
  "Color Balance RGB" instead.

- Convert the zoom widget on the navigation window to a standard
  drop-down for better fitting the darktable style.

- Lower the ISO 12646 border size still staying in the recommended
  size.

- Rework the preset dialog filter to better show the relation between
  the raw, non-raw and the three formats HDR, monochrome and
  color. This avoids confusions and creating presets that are actually
  not applied.

- The default module group has been removed. It is better to use one
  of the scene-referred group.

- Add support for loading QOI and FITS images.

- Add support for writing metadata to XCF format (see notes below).

- Read Exif metadata from AVIF, HEIC and JPEG XL images using native
  libraries if Exiv2 does not support it.

- Write Exif data to the eXIf PNG chunk if using Exiv2 version newer
  than 0.27.x. This is the new standard way to store Exif data in PNGs.

- Export masks for EXRs as extra channels.

- Re-enable loading of BigTIFF images by attempting the native
  libtiff-based reader first.

- Redesign the export and thumbnails generation.

  Some hacks have been accumulated to try to have exact size of the
  exported pictures. All this has been redesigned and simplified to have
  a better export size.

- Improve Highlights reconstruction "inpaint opposed" performance by
  providing an OpenCL implementation and using internal caching in
  darkroom.

- Various improvements of the debug interface:

  - -d common outputs most valuable information and should be used for
     bug reports instead of -d all.

  - --bench-module <modulea,moduleb> does runtime benchmarking of the
      specified modules.

  - --dump-pipe <modulea,moduleb> writes in & output data of the
    specified modules as pfm files for inspection.

- Improve support for Lens correction based on embedded data.

  - Add supports for dng files,
  - Allows finetuning of scale,
  - Add an auto-scale button.
  - Improve overall module performance of about 8%.

  - An improved algorithm for embedded metadata lens correction has
    been added to the lens module. It improves distortion and vignetting
    corrections for the supported Fujifilm and Sony images.

    Also add support for Fujifilm X-Trans I/II/III cameras raw files.

  - New sliders for changing the image scaling and chromatic aberration
    fine tuning has been added in the lens correction module.

- Added section headers to the sort by drop-down (files, times, etc).

- Shortcuts assigned to presets or styles will be shown when hovering
  over them in their menu.

- Long left clicking a preset will keep the menu open so you can
  quickly switch between several to see the effect without having to
  repeatedly click the preset button to reopen the menu. You can also
  scroll over the preset button to switch to previous/next presets
  (like you already could via shortcuts).

- When the crop module receives focus and switches to an uncropped
  view of the image, the crop areas around the edges of the image
  briefly light up to indicate that they are now draggable.

- The crop module, which shows the full image to facilitate making
  adjustments, will not trigger an unnecessary recalculation until the
  module loses focus (for example by switching to another module or by
  collapsing the crop module itself) at which point the new crop will
  be used to resize. If shortcuts are used to make changes to the crop
  without focusing the module, they will still be implemented
  immediately.

- The height of resizeable widgets and lists can now be changed by
  dragging their bottom. The previous method to achieve this, by
  scrolling while holding the control key, has been changed to
  shift+alt+scroll (and a note has been added to all tooltips
  consistently). This frees up <kbd>ctrl+scroll</kbd> to fine-tune
  changes in RGB Levels or the histogram (to change exposure or black
  level). In the navigator ctrl+scroll will adjust zoom level without
  bounds, like it would do over the central area.

- The histogram gui has been reworked. Control buttons have been
  splitted in two groups: on the left side, a series of buttons to
  switch among histogram modes (histogram, waveform, rbg parade,
  vectorscope). On the right side, the buttons that control the aspect
  of each mode (RGB Channels, Orientation, Vectorscope type). For the
  RYB vectorscope, a series of buttons have been added to visualize
  guide lines for the most common color harmonies.

- The central image internal scroll zoom logic in the darkroom has
  been reworked in order to make the zoom steps more perceptually
  uniform for all the image sizes.

- Better display of the module's instance name in darkroom to have a
  clear visual separation between the module name and the instance
  name. The label name in the history is also updated accordingly.

- Improve display of the range rating widget. It should be more
  readable with a better contrast and a rework of some icons.

- In lighttable, link "hold" and "sticky" preview shortcuts to the
  same action (previously there were two "toggle sticky preview mode"
  actions, one with and one without focus detection). Focus detection
  is selected via an element, hold or toggle via an effect. All mapped
  shortcuts are shown in the tooltip of the preview layout button.

- Makes laplacian highlights recovery mode less memory hungry (save
  around 40%) and allow for a large speed up. This makes this recovery
  mode lot more usable and allows for more recovery iterations.

- Add support for embedded DNG lens corrections metadata. By reading
  the rectilinear and vignette_radial metadata it is now possible to
  make lens correction.

- Add support for MaxApertureValue metadata to complement the already
  supported ApertureValue. This is the only metadata tag available in
  Leica M Monochrom, M8, M9 & M10 DNGs.

- The search filter has been improved to also search the camera's brand
  and model.

- A full copy and paste is always done in overwrite mode now. Using
  the append mode was in the vast majority of time wrong in this
  case. When the full history is copied and pasted into another image
  we do want to just use the new history as a replacement of the
  previous one. It makes no sense adding some multi-instance for some
  modules for example. So this new default should fit better in the
  workflow.

- The brush path is now a bit more transparent to better see what is
  actually painted.

- Style tooltip immediately shows module details while waiting for
  preview image to be calculated.

- In the style and copy/paste dialog a new column display the module
  masking status. If any mask (draw, parametric or raster) is used the
  column contains a mask icon.

- Makes the drawn mask tools' tooltip of Liquify module consistent
  with the other mask tools as found in the blending drawn masks.

- Removed "Demosaicing for zoomed out darkroom mode" configuration
  option. This option is not useful now that the pixel-pipe cache
  has been improved. It could also potentially lead to slight
  differences.

- In mask manager some actions in the menu could be activated even
  though they were a no-op given the context. So now the move up/down
  action are disabled for the first and last element in a group
  respectively. The setting of the mask operator is disabled for the
  first element in a group. Basically those are small UI improvements.

- Two new sharpness' presets on the diffuse or sharpen module have
  been added. One standard sharpness and one with a stronger effect.

- The snapshots buttons have been redesigned to have a better look. The
  display is now closer to the history module. At the same time the
  module label is displayed and it is editable with
  <kbd>ctrl+click</kbd>.

- Read focus distance for Nikon Z bodies.

- Change reading creator metadata from IPTC Information Interchange
  Model to prefer By-line over Writer/Editor. Read date/time and
  description metadata from commonly used properties.

- The drawing of arrow between the source and destination area in the
  Retouch module has been reworked. We have a arrow for all forms
  (instead of a simple line for Brush & Path shapes). The arrow is
  also always the smallest one between both area and so avoid crossing
  the shapes. Finally for the circle and ellipse the arrow doesn't
  start anymore from the center area but as for other shapes on the
  border.

- Added the option to change the zoom behavior for the middle mouse
  button in darkroom. The setting can be enabled in the preference
  dialog under 'darkroom' category. The new behavior will only zoom
  between fit and 100%. To have 200% zoom a <kbd>ctrl+click</kbd> is
  needed.

- The mask shape size/feather/hardness sliders in the masks manager
  module now display in log scale and scrolling over them makes
  relative adjustments, just like <kbd>Shift</kbd> scrolling over the
  shape itself. <kdb>Ctrl</kbd> or <kdb>Shift</kbd> will make fine or
  coarse adjustments, also with shortcuts if fallbacks are
  enabled. Shortcuts assigned to the sliders can be used to adjust
  brush size/hardness while drawing.

- Improve the mask editing by making it easier to select masks'
  control points and more specifically the path's segments. In some
  cases it was difficult to select a segment and instead the whole
  path mask was selected and so moved. The on canvas drawing of the
  masks has also been improved to be more consistent for all kind of
  forms.

- Added a 5th mode for combining mask shapes: "sum". This allows
  repeated brush strokes with low opacity layered on top of each other
  to increase the strength of the mask. This is now the default for
  brush shapes.

- The mode for all selected shapes can be changed in one go from the
  right-click menu in the mask manager.

- When using a shortcut to add shapes in the blending section of a
  module, the blending mode will switch to "drawn mask" or "drawn &
  parametric mask", depending on what it was before, so that any newly
  created shape will actually have an effect.

- Show the full-frame equivalent focal length and crop factor in the
  image information module.

- Added options in the watermark module for more fine-grained control
  of the watermark scaling. In conjunction with the new 'fixed-size-text'
  template it is now possible to insert text with constant font size.

- Make successive changes to sliders (for example by dragging,
  scrolling or shortcuts) and other widgets more responsive by
  creating fewer undo records. This also makes using undo more
  effective because it doesn't step through every micro change.

- Improve the ISO range selection widget for the auto apply presets
  dialog.

- In the drawn mask blending mode there was in addition to the "toggle
  polarity" - found in all blending modes - the combo box "invert
  mask". Both where doing the very same thing so the later has been
  removed.

- Support the encoder ring and button lights of the Behringer X-Touch
  Compact via midi. Unmapped encoder presses fall back to reset the
  encoder.

- Midi buttons mapped to the reset effect of a slider or combo (either
  directly or via fallback, like the row below the faders of the
  X-Touch Compact) light up if the current value is not the default.

- Resetting a combo (by double clicking or via a shortcut) that has
  sub headers will now select the first selectable item.

- Image change requests in the darkroom (space/backspace/click in
  filmstrip) used to be quietly ignored if a recalculation was
  currently ongoing. Now, they will be processed as soon as the pipe
  is ready. Any further changes that were made to the previous image
  while waiting will be discarded.

- Enable per-color black point manual adjustment for non-CFA
  (a.k.a. linear) raw images. Note that file embedded levels might
  still not be set automatically on import.

- Enhance dithering module with posterization modes and masking, and
  rename it "dither or posterize" to make the new functionality more
  easily discoverable.

- Add Help buttons to several dialogs and preferences tabs to directly
  launch the online manual.

- New version of Fimic color science v7 (2023) which has been set as
  the default. The color preservation mode has been removed and a new
  slider is proposed to control the highlights saturation. This slider
  give a mix between the max RGB chroma preservation mode and the no
  preservation mode.

- Add support for some more metadata keys to be imported:
  - Iptc.Application2.Byline
  - Iptc.Application2.DateCreated
  - Iptc.Application2.TimeCreated
  - Exif.Image.ImageDescription

- The Shadows and Highlights module is now using the Bilateral filter
  by default has it will avoid most of the haloing making it a better
  default.

- Add some standard print sizes (5x7, 8x10, 11x14) to the list of
  predefined aspect ratios in Borders module.

- Make all remaining color-picker buttons accessible via shortcuts and
  lua.

- Added tooltip to edges of sliders with soft limits on how to set
  values outside those boundaries.

- Reverse the sort order of the shapes in mask manager groups so that
  the lowest ranking shape is at the bottom of the group. For
  consistency the sort order of shapes outside of a group has been
  changed.

- Add some new aspect ratios in the border module:
  - CinemaScope
  - US Letter
  - US Legal

- Improve clarity and usability of dialog to confirm further action
  upon failure of physical file deletion or moving to trash.

- The dropdowns to select the amount of brush smoothing and whether
  pen pressure affects brush size, hardness or opacity have been moved
  from the preferences dialog to the "properties" collapsible section
  in the mask manager, so they can be changed while drawing or even
  assigned shortcuts.

- Add metadata display of the current image's embedded ICC profile to
  the input profile widget's tooltip.

- Don't push a trouble message if multiple channel-mixer modules are
  used with applied masks. This usage is expected to cover different
  areas and so should be considered as correct.

- Add geometry of Spyder Checkr Photo in ColorChecker RGB module.

## Bug Fixes

- Fix the reset of the sort order to 'filename' on every collection
  change.

- Remove the commit button from the crop module as it was not used
  anymore.

- Fix the reset of modules with specific default parameters to ensure
  that the modules will be set back in the same state as it was when
  first importing the image. This fix is related with the rework of
  auto apply module default parameters in the section above.

- Properly transform XMP regions from metadata to ensure they match
  the image. The XMP regions may come from the camera face recognition
  for example.

- Fix some rounding issues in the calculation of the borders in the
  border module. This creates borders on opposite sides with the same
  size.

- Code maintenance and bugfixes for writing dng files as used in
  "Create HDR"

- Pixelpipe cache safety and performance improvements. This makes
  better hit when looking in the cache and so allows for better
  performance.

- Fix some pixel-pipe cache issues related to mask visualization and
  module's internal histogram (like RGB Curve for example). This
  ensures better hit in the cache leading to better performance and
  also avoids some refresh issues in some cases.

- Fix "--threads n" restricts OMP threads to specified number (does
  not allow for more threads than available on the host.

- Raw chromatic aberration module always works on full image data
  so quality is immune to scaling in darkroom mode.

- A bug with shortcuts (and dt.gui.action) to set the active item in a
  combobox with varying content has been fixed and now it is also
  possible to directly set the values of the combos for the focused
  module's blending mode etc. (by setting the shortcuts effect).

- Fix the update preset entry in the presets menu to allow for it to
  be activated in more situations. For example after entering in the
  darkroom and modifying some parameters in the module the update
  preset entry was not selectable. It was necessary to first select
  the preset and then changing the parameters.

- Fix the color picker sample area calculation to ensure at least one
  pixel is selected. At large zoom level and with a very small area
  some rounding were sometimes returning an empty area. This leads to
  returning a wrong color sample.

- Fix the ignore EXIF rating on initial import for images containing
  the XMP.xmp.Rating tag. This does not change the rating if an XMP
  with some specific rating has been already made.

- Fix some minor memory leaks in some modules.

- Fix a possible crash when selecting the original module history
  state and compressing the history.

- Fix a possible crash in gradient mask creation due to an issue in
  the implemented parallelism.

- Fix crawler when updating the XMP. The XMP file must be set with the
  timestamp of the database entry to ensure we don't have continuous
  mismatches reported.

- Fix different issues on the mask manager. It is now possible for all
  mask kinds to be added continuously. Also the brush was not properly
  displayed after being created from the mask manager. A crash when
  creating gradients from the mask manager has been fixed. For all
  shapes the editable state is properly set after being created making
  it possible to move and resize the different parts.

- Fix brush correction tool placement as used in Retouch module, the
  issue was more visible when the image is distorted.

- Fixed CPU vs OpenCL output differences in PPG and VNG/VNG4
  demosaicers, match greens and color smoothing.

- Finalscale now properly uses same user defined scaling mode for
  image and masks.

- Fix display when editing a shapes' name in masks manager. Avoid
  overlay of the old and new name.

- Fix for clean Nikon camera make and model Exif info on import;
  opening in darkroom is no longer required, and now works for non-raw
  files as well.

- Fix Canon CR3 metadata crop not being ignored; full visible sensor
  area (as determined by LibRaw) is always being used now on new
  imports.

- When using Wayland give priority to XWayland, because native Wayland
  is the cause of many issues in darktable.

- When using the spot exposure mapping mode, properly reset the mode to
  "correction" when changing image.

- Fix a bug where the "highlights reconstruction" module, which was
  not applicable to the current image, could be enabled. For example
  it is now impossible to enable the module if the image is a JPEG as
  the module works only with RAWs.

- Fix border issue in inpaint opposed highlights reconstruction. Some
  pixels on the border of the image where not handled by the
  algorithm. This may lead to a small difference on the border of the
  image and will avoids some possible redish borders.

- Fix issue in highlights reconstruction segmentation algorithm where
  the mask display could be broken due to accessing some non
  initialized data.

- Avoid XMP writing if not requested and image was not altered. This
  rule is properly followed now also when importing RAW + JPEG.

- Make sure the database timestamp is always set when possibly writing
  a sidecar xmp file.

- A workaround was implemented for the mouse hover effect over sliders
  and dropdowns, that used to cause the whole side panel, including
  histogram, to be redrawn on each move between widgets, reducing cpu
  consumption.

- Fix operator state in the mask manager. When moving up/down a mask
  we ensure that the first mask has no operator and that the second
  one has an operator assigned. If no operator has been set yet the
  default union operator is assigned.

- In rotate & perspective, if the current rotation is close to +-180
  degrees, adjusting it by drawing a horizon line with
  <kbd>right-click+drag</kbd> could lead to it being clipped at the
  end of the slider. It now correctly wraps around and a manually
  entered value outside the range, like 182, will be wrapped as well,
  to -178 degrees.

- Fixed monochrome images loading.

- Fix tiny circle mask display. Ensure that the mask is always
  visible.

- Fixing OpenCL library loading in case of not fully implemented
  required symbols.

- Rework the masks drawing to ensure all masks are drawn the same
  way. The central area, border and highlighted segments are now
  displayed consistently. The highlighted segment is more visible,
  this is especially true for the brush mask where the highlighted
  segment was barely distinguishable due to a bug.

- Set imported EXR image size to the extent of valid data window only.

- Properly translate collection sort names in the recent collection
  sort history pop-up.

- Fix dual demosaicing options for 4-color Bayer sensor cameras where
  only VNG4 and PassThrough are supported.

- Do not truncate focal length on thumbnails to avoid loss of
  precision of displaying values.

- Let exposure module to set neutral settings for non-RAW images.

- Fixing details masks while switching to darkroom which could lead to
  a crash.

- Fixing feathering masks in certain modules (Lens, Retouch, Liquify,
  Spot removal).

- Fix some rare cases where masks are not displayed when trying to
  edit them after just starting darktable or changing of module group.

- Fix slideshow not working properly on HiDPI displays.

- Fix crashes while using raster masks after reordering the pixelpipe.

- Fix details mask for images in blown-out parts of the image.

- Allow adding color patch on 7x7 grid of the Color Checker module.

- Feathering input fixed while using distorting modules like retouch
  or lens.

- Fix a long-standing potential memory bug in interpolation code,
  though one which never occurred due to how that code is used in
  darktable.

- Rework the handling of the metadata editor to prevent possible data
  loss.

- Fix import of auto-applied presets where the upper bound of ISO,
  aperture and exposure could be set as the lower bound.

- Fix entering custom aspect ration in the border module.

- Fix pin icon update in filtering module which could crash darktable
  when using some specific filter combinations.

- Fix rating toast message not shown when rating a collapsed group
  of images using keyboard shortcut.

- Fix possible crash in astrophoto denoise when used on CPU (no
  OpenCL).

- Some minor fix of Spyder Checkr 48 (v2 - after 2018) reference
  values.

- Fix possible crash in the Edge-Avoiding a-trous Wavelet when
  handling very small image's regions.

- Properly ignore empty GPX latitude/longitude which would otherwise
  create bogus location coordinates.

- Fix position saving in collect module history and recent-collection
  module. This ensure that activating an history entry restore
  thumbnail offset like it was when the entry has been saved.

- Avoid a possible (and rare) case of images flipping in the print
  view due to some preview being updated.

## Lua

### API Version

- API version is now 9.1.0

### Add action support for Lua

- The lua call to dt.gui.action has become more flexible, with most
  parameters optional, so you can read the focused status of a module
  doing just dt.gui.action("iop/filmicrgb", "focus").

- Tooltips show the compact lua commands in mapping mode (only
  adding the last parameter, instance, if the module supports
  multi-instance) and have been added to presets and styles menus as
  well.

- Lua commands can be copied to the clipboard, using ctrl+v in the
  shortcuts dialog, both from a selected action or shortcut, or long
  right click when in mapping mode (over a widget) or in the
  presets/styles menus.

- The shown/copied lua command for a slider or combobox will set the
  value it currently has.

- A shortcut can now be directed to a lua script that mimics a
  standard slider, dropdown or button, but dynamically selects the
  real widget(s) that receive(s) it, based on for example which module
  is focused or enabled. The advantage is that all fallbacks work as
  normal, so you can assign a midi knob to it and turning it (holding
  shift/ctrl to speed up/down) or pressing it to reset work regardless
  of which widget receives it. Basically this is a much more flexible
  alternative to the fake widgets under processing
  modules/<focused>. This allows owners of for example an x-touch mini
  to use their scarce rotors in different, fully configurable, ways
  while working in different modules (which can also be focused using
  midi buttons which will then light up). Such configurations could be
  shared using https://github.com/darktable-org/lua-scripts.

- Support shortcuts to sliders/combos created in lua, either via
  visual mapping mode or in the shortcuts dialog under the lua
  category. Elements and effects are not supported.

### Other Lua changes

- Added aspect_ratio field to dt_lua_image_t for image orientation
  retrieval support.

- dt_lua_image_t now repects the `show time in milliseconds` setting
  in lighttable preferences and will return exif_datetime_taken with
  milliseconds when enabled.

- Added final_height, final_width, p_height, and p_width fields to
  dt_lua_image_t.

- Two new properties have been added to get the flags (category,
  private) and the synonyms from a tag.

- Moved `pixelpipe-processing-complete` event from the end of the
  image pixelpipe to the end of the preview pixelpipe to catch
  completion of events that only update the preview such as
  spot exposure measurement in the exposure module.

## Notes

- When exporting to AVIF, EXR, JPEG XL, or XCF, selecting specific
  metadata (e.g. geo tag or creator) is not currently possible. For
  AVIF, EXR, JPEG XL, and XCF formats, darktable will not include any
  metadata fields unless the user selects all of the checkboxes in the
  export preference options.

- In order to support the correct display of numbers in darktable, the
  minimum supported Gtk version has had to be increased to
  3.24.15. For people who need to build darktable with an older
  version, this can be supported by reverting the following change:
  remove line 241 of darktable.css file on your system. See:
  https://github.com/darktable-org/darktable/issues/13166

- Beginning with this release a new support policy regarding macOS versions will be in place.
  darktable releases will just support macOS versions that are also supported by Apple.
  So release 4.4 drops support for macOS versions older than 11.3.

## Changed Dependencies

### Mandatory

- ???

### Optional

- ???

## RawSpeed changes


## Camera support, compared to 4.2

### Base Support


### Missing Compression Mode Support

- Fujifilm lossy
- Nikon high efficiency
- Sony lossless

### White Balance Presets

- Canon EOS R7
- Canon EOS R10

### Noise Profiles

   No data for this release.

### Suspended Support

No samples on raw.pixls.us

- Creo/Leaf Aptus 22(LF3779)/Hasselblad H1
- Fujifilm FinePix S9600fd
- Fujifilm IS-1
- GoPro FUSION
- Kodak EasyShare Z980
- Leaf Aptus-II 5(LI300059)/Mamiya 645 AFD
- Leaf Credo 60
- Leaf Credo 80
- Minolta DiMAGE 5
- Olympus SP320
- Panasonic DMC-FX150
- Pentax Q10
- Phase One IQ250
- Samsung GX10
- Samsung GX20
- Samsung EK-GN120
- Samsung SM-G920F
- Samsung SM-G935F
- Sinar Hy6/ Sinarback eXact
- ST Micro STV680

## Translations

- New English translation with capital letters
- ???
