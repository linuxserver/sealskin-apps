# SealSkin Application Store

This repository contains the `apps.yml` file, which serves as the primary application manifest for the **SealSkin** file isolation stack. It defines the applications available to users, their configurations, and the file types they are associated with.

## File Structure

The YAML file is organized into two main sections at the root level: `extension_groups` and `apps`.

### `extension_groups`

This section is designed to keep the manifest clean and maintainable by defining reusable "buckets" of file extensions.

It uses standard YAML **anchors** (`&`) to define a group and **aliases** (`*`) to reuse it later in the `apps` section. This prevents you from having to repeatedly type common lists of extensions (e.g., `jpg, jpeg, png, gif`) for every image-related application.

**Example:**
```yaml
extension_groups:
  common_image_files: &common_image_files
    - jpg
    - jpeg
    - png
    - gif
    - bmp
```
Here, `&common_image_files` creates an anchor that can be referenced later using `*common_image_files`.

### `apps`

This is a list that contains the definition object for every application available in this SealSkin App Store. Each object in the list defines one application and its properties.

#### Application Object Fields

Each application is defined by a set of key-value pairs:

*   `id`: (String) A unique, lowercase, URL-safe identifier for the application.
*   `name`: (String) The human-readable name of the application that will be displayed in the UI.
*   `logo`: (String) A URL pointing to the application's logo.
*   `url`: (String) A URL pointing to the application's source code or Docker Hub page for reference.
*   `provider`: (String) The backend provider for launching the app.
*   `provider_config`: (Object) A nested object containing configuration specific to the provider. For Docker, this includes:
    *   `image`: (String) The full Docker image name and tag (e.g., `lscr.io/linuxserver/gimp:latest`).
    *   `port`: (Integer) The internal port that the application's web service listens on inside the container.
    *   `nvidia_support`: (Boolean) Set to `true` if the application can leverage NVIDIA GPUs for hardware acceleration.
    *   `dri3_support`: (Boolean) Set to `true` if the application can leverage Intel/AMD GPUs via DRI3 for hardware acceleration.
    *   `type`: (String) The category of the application. Can be `app`, `browser`, or `desktop`.
    *   `url_support`: (Boolean) Set to `true` if the application can be launched with a target URL (e.g., a web browser).
    *   `open_support`: (Boolean) Set to `true` if the application can be launched by opening a file. This is required for the `extensions` list to be meaningful.
    *   `extensions`: (List of Strings) A list of file extensions (without the leading dot) that this application can open. It is **highly recommended** to use aliases (`*group_name`) from `extension_groups` to populate this list.
