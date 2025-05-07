
# Percepio Trace Exporter

The **Percepio Trace Exporter** is an Eclipse plugin designed to seamlessly integrate your Eclipse-based IDE with [Percepio Tracealyzer](https://percepio.com/tracealyzer/). This integration facilitates the capture of snapshot trace data from embedded applications, enhancing debugging and performance analysis.

## Features

- **Snapshot Trace Capture**: Easily capture snapshot traces from your embedded applications during debugging sessions.
- **IDE Integration**: Integrates with Eclipse-based IDEs, including STM32CubeIDE, to provide a streamlined workflow.
- **Debug Probe Compatibility**: Supports any debug probe compatible with your existing toolchain.
- **Cross-Platform Support**: Available for Windows, macOS, and Linux/GTK platforms.

## Installation

### Option 1: Eclipse Marketplace

1. Open your Eclipse-based IDE.
2. Navigate to `Help` > `Eclipse Marketplace...`.
3. In the **Find** field, enter `Percepio Trace Exporter`.
4. Click **Install** next to the plugin entry.
5. Follow the installation prompts and restart the IDE when prompted.

### Option 2: Update Site

1. Open your Eclipse-based IDE.
2. Go to `Help` > `Install New Software...`.
3. In the **Work with** field, enter the update site URL: `https://percepio.com/exporter`
4. Press **Enter** and wait for the plugin list to populate.
5. Expand the **Percepio** category, select **Percepio Trace Exporter**, and click **Next**.
6. Follow the installation prompts and restart the IDE when prompted.

## Usage

1. Start a debugging session in your Eclipse-based IDE.
2. Once the application is halted at a breakpoint, navigate to `Percepio` > `Save Snapshot`.
3. The captured snapshot will be saved and can be opened with Percepio Tracealyzer for detailed analysis.

## Requirements

- **Eclipse Versions**: Compatible with Eclipse versions 4.3 (Kepler) through 4.19 (2021-03).
- **Java Runtime**: Ensure that your Java Runtime Environment supports the necessary cryptographic extensions. If you encounter issues related to encryption during installation, consider installing the Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files.

## License

This plugin is distributed under the [BSD License](https://opensource.org/licenses/BSD-3-Clause).

## Support

- **Official Website**: [https://percepio.com](https://percepio.com)
- **Tracealyzer**: [https://percepio.com/tracealyzer/](https://percepio.com/tracealyzer/)
- **Eclipse Marketplace**: [Percepio Trace Exporter](https://marketplace.eclipse.org/content/percepio-trace-exporter)

For further assistance or to report issues, please contact [support@percepio.com](mailto:support@percepio.com).

*Note: This README is based on information available as of January 15, 2021. For the most recent updates and support, please refer to the official Percepio website.*
