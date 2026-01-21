Stirling-PDF Podman User Quadlet
================================

**Disclaimer:** I am not affiliated with, associated, authorized, endorsed by, or in any way officially connected with the Stirling-PDF project.  Back up important data before proceeding.

This guide demonstrates how to deploy **Stirling-PDF** as a **User (Rootless) Quadlet**. Stirling-PDF is a robust, local-hosted PDF manipulation tool.

FAT vs. Regular Distro
----------------------

This Quadlet uses the `latest-fat` image. Here is the difference:

*   **FAT (Recommended):** Includes all OCR languages and advanced dependencies (Python, LibreOffice) out of the box. It is larger but ensures all features work immediately.
*   **Regular:** A much smaller image. It downloads dependencies at runtime when needed. This can lead to slower performance during the first use of certain tools.

Memory Management & Optimization
--------------------------------

**Note on Memory:** PDF processing (especially OCR on large files) is extremely memory-intensive.

In this configuration, we have tuned the memory in two places:

1.  `--memory=8G`: This tells Podman to limit the container to 8GB of system RAM.
2.  `-Xmx4g`: This tells the Java Virtual Machine (inside the container) that it can use up to 4GB for its "Heap."

Leaving a gap between the Java limit (4G) and the Container limit (8G) is standard practice to allow room for the OS-level dependencies (like OCR engines) to run alongside the main Java app without crashing the container.

1\. Installation & Setup
------------------------

`mkdir -p ~/.config/containers/systemd`
`mv stirling-pdf.container ~/.config/containers/systemd/`

2\. Activation
--------------

Run the following commands to start the service as your local user:

`systemctl --user daemon-reload`
`systemctl --user start stirling-pdf`
`systemctl --user enable stirling-pdf`

3\. Verification
----------------

Check the logs to ensure Java initialized correctly within the memory constraints:

`journalctl --user -u stirling-pdf -f`

Access the tool at: `http://[Your-IP]:8180`

4\. Maintenance
---------------

`podman auto-update`
