# Extension support

> With the introduction of Extensions in Blender 4.2, the old way of creating add-ons is considered deprecated.

[Extensions](https://docs.blender.org/manual/en/4.2/advanced/extensions/getting_started.html) are supported.
For general comparison visit [Legacy vs Extension Add-ons](https://docs.blender.org/manual/en/4.2/advanced/extensions/addons.html#legacy-vs-extension-add-ons).
VS code uses the automatic logic to determine if you are using addon or extension:

| Blender version | Addon has `blender_manifest.toml` | Development location:  | Your addon is interpreted as: | Link is created?                                   |
| --------------- | --------------------------------- | ---------------------- | ----------------------------- | -------------------------------------------------- |
| Pre 4.2         | Does not matter                   | ADDON_DEFAULT_LOCATION | Legacy addon                  | No link is created                                 |
| Pre 4.2         | Does not matter                   | Anywhere on disk.      | Legacy addon                  | You addon is linked to ADDON_DEFAULT_LOCATION      |
| 4.2 and above   | True                              | Anywhere on disk.      | Extension                     | You addon is linked to EXTENSIONS_DEFAULT_LOCATION |
| 4.2 and above   | True                              | ADDON_DEFAULT_LOCATION | Legacy addon                  | No link is created                                 |
| 4.2 and above   | True                              | ANY_REPO_LOCATION      | Extension                     | No link is created                                 |
| 4.2 and above   | False                             | Does not apply.        | Legacy addon                  | You addon is linked to ADDON_DEFAULT_LOCATION      |

There is no setting to override this behaviour. Find out your default paths:
```python
>>> ADDON_DEFAULT_LOCATION = bpy.utils.user_resource("SCRIPTS", path="addons")
'/home/user/.config/blender/4.2/scripts/addons'
>>> ANY_REPO_LOCATION = [repo.custom_directory if repo.use_custom_directory else repo.directory for repo in bpy.context.preferences.extensions.repos if repo.enabled]
['/home/user/.config/blender/4.2/extensions/blender_org', '/home/user/.config/blender/4.2/extensions/user_default', '/snap/blender/5088/4.2/extensions/system']
>>> EXTENSIONS_DEFAULT_LOCATION = bpy.utils.user_resource("EXTENSIONS", path="user_default")
'/home/user/.config/blender/4.2/extensions/user_default' 
```

## Examples

I am using Blender 3.0 to develop my addon in `/home/user/blender-projects/test_extension/`.
My addon supports addons and extensions (has both `bl_info` defined in `__init__.py` and `blender_manifest.toml` file.)
  - Result: my addon is interpreted to be addon (because Blender 3 does not support extension). My addon is linked to ADDON_DEFAULT_LOCATION
 
I am using Blender 4.2 to develop my addon in `/home/user/blender-projects/test_extension/`.
My addon supports addons and extensions (has both `bl_info` defined in `__init__.py` and `blender_manifest.toml` file.)
  - Result: my addon is interpreted to be extension. My addon is linked to EXTENSIONS_DEFAULT_LOCATION