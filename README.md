# ASCII Art Generator

A robust Java-based Command Line Interface (CLI) application that transforms standard images (JPG, PNG) into artistic ASCII representations. This project utilizes luminance-based brightness mapping, efficient data structures for character matching, and a modular package-based architecture.

---

## 🏗️ System Architecture

The project is structured into distinct packages to ensure a separation of concerns and maintainable code:

### **`ascii_art`**

* **`Shell`**: The primary controller that manages user input and orchestrates the art generation process. It utilizes a `SubImgCharMatcher` to manage the active character set and interfaces with `Image` and `AsciiArtAlgorithm`.
* **`AsciiArtAlgorithm`**: Implements the core conversion logic, coordinating data from the image and the character matcher to produce the final grid.
* **`UsageException`**: A custom exception class used to handle and report logical errors in user commands.
* **`KeyboardInput`**: A utility class provided for reading user input streams.

### **`image`**

* **`Image`**: Represents the image file in memory, providing pixel-level data access.
* **`ImageProcessing`**: Prepares the image by partitioning it into sub-images and calculating the average luminance/brightness of each segment.

### **`image_char_matching`**

* **`SubImgCharMatcher`**: Matches specific brightness values to characters. It calculates initial character brightness using the provided `CharConverter`.
* **`CharConverter`**: A utility that converts characters into a grid of pixels for visual weight analysis.

### **`ascii_output`**

* **`AsciiOutput` (Interface)**: Defines the standard for outputting the generated art.
* **`ConsoleAsciiOutput`**: Implements the interface to render art directly in the terminal.
* **`HtmlAsciiOutput`**: Implements the interface to export the art as a high-quality HTML file.

---

## 📊 Data Structures & Performance

To ensure the rendering process remains efficient even at high resolutions, the following data structures were utilized:

| Structure | Component | Average Complexity | Justification |
| --- | --- | --- | --- |
| **`HashMap`** | `SubImgCharMatcher` |  O(1)| Used for rapid storage and retrieval of pre-calculated character brightness data. |
| **`TreeMap`** | `SubImgCharMatcher` |  O(logn)| Enables the use of `floorKey` and `ceilingKey` to find the character with the brightness level closest to a target value. |
| **`TreeSet`** | `SubImgCharMatcher` / `Shell` |O(logn)  | Manages characters with identical brightness, ensuring they are sorted by ASCII value for deterministic selection. |
| **`2D Arrays`** | Core Logic | O(1) | Used for pixel data storage and image grid manipulation to provide constant-time access. |

---

## ⚠️ Exception Handling & Logic

* **Custom Exceptions**: The `UsageException` class is used to wrap logical errors such as incorrect command formats or resolution boundary violations.
* **Decoupled Error Reporting**: Logic for detecting errors is separated from the printing logic. Helper methods throw exceptions which are then caught by the main `run` loop in the `Shell` to provide user-friendly feedback without crashing the program.
* **Graceful Termination**: File-related errors (`IOException`) are caught during image loading to inform the user and terminate the program safely.

---

## 🔄 Key Design Choices

* **Single Source of Truth**: We implemented the `getCharSet()` method in `SubImgCharMatcher`. This allows the `Shell` to access the active characters for both display and processing without maintaining a redundant (and potentially inconsistent) list.
* **Extensibility**: By following the `AsciiOutput` interface and using a modular algorithm, the system can easily be extended to support new output formats or conversion methods.

---

### **Project Information**

* **Course**: Object-Oriented Programming (67109)
* **Institution**: Hebrew University of Jerusalem
* **Authors**: Amit Tzur & Zohar Mattatia
* **Date**: 2025

