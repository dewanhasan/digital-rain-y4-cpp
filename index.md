---
Name: Dewan Hasan
id: g00410156
---


## **Introduction**

This project aims to recreate the Matrix-style digital rain effect in a modern C++ console application. Using object-oriented programming (OOP), we structure the simulation into modular components: DigitalRain controls the animation, while RainDrop manages individual falling elements. The simulation features smooth animations, randomized movement, and scalable design, utilizing multi-threading and efficient console rendering. This documentation provides insights into the implementation, functionality, and potential improvements.


## **Design & Test**

### **Log 1: First Attempt**

In my initial attempt at the Digital Rain simulation, I created a simple C++ program that randomly placed the word "Hello" at different positions in the console. The implementation used GotoXY() to move the cursor and rand() to generate random coordinates within the console's width and height. However, instead of cascading rain-like movement, the words appeared randomly and stacked up, creating a scattered effect rather than a falling animation. This provided a useful starting point for understanding console manipulation and random positioning, but the next step is to implement a continuous downward motion to better simulate the rain effect.In my initial attempt at the Digital Rain simulation, I created a simple C++ program that randomly placed the word "Hello" at different positions in the console. The implementation used GotoXY() to move the cursor and rand() to generate random coordinates within the console's width and height. However, instead of cascading rain-like movement, the words appeared randomly and stacked up, creating a scattered effect rather than a falling animation. This provided a useful starting point for understanding console manipulation and random positioning, but the next step is to implement a continuous downward motion to better simulate the rain effect.

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/GetImage.png?raw=true" width="400" height="300">

### **Log 2: Structuring the Digital Rain Simulation**

In this iteration, we restructured the Digital Rain simulation to follow modern C++ principles, ensuring modularity, maintainability, and efficiency. The project now consists of multiple components, each handling a specific aspect of the simulation:

1. DigitalRain Class
   - Manages the **overall simulation**, handling multiple rain streams and rendering them in the console.
   - Uses `GotoXY()` to control cursor positioning for smooth display updates.
   - Calls `generateRain()` to continuously update and display the animation.
  
     
2. RainDrop Class:
   - Represents an **individual rain stream**, tracking its position and movement.
   - Implements a `fall()` function to simulate downward motion.
   - Uses `get()` to return the current rain characters (`0` or `1`), allowing `DigitalRain` to render them.

3. Test Class:
   - A placeholder for **future debugging and validation** .
   - Can be expanded to include performance tests and rendering accuracy checks.

Each class has its own header (.h) and implementation (.cpp) files, ensuring proper encapsulation and separation of concerns. This makes the project scalable and easier to maintain.
  
However, the initial implementation didn't work as expected. The numbers were falling downwards, but they kept stacking up instead of creating a continuous rain effect. This happened because I forgot to clear the screen between frames. To fix this, I added **system("cls");** at the beginning of the loop to clear the console before redrawing the characters. This simple change allowed the animation to refresh properly and create the desired falling effect.

Before using **system("cls");**

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/Log2a.png?raw=true" width="400" height="300">

After using **system("cls");**

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/Log2b.png?raw=true" width="400" height="300">



By adopting modern C++ features such as modular design, efficient console manipulation, and structured class interactions, this update enhances the project's **scalability and flexibility**. The next step involves refining the animation, optimizing performance, and exploring color effects and customizable character sets.


### **Log 3: Fixing Flickeing Screen**

After implementing the `system("cls");` to the system, I noticed that even though it clears the screen and prevents stacking, but the screen keeps flickering as it is clearing the screen every 2 seconds, Which is really not ideal for the animation.

To eliminate flickering, I replaced `system("cls")` with a better approach:

   - Clearing only the previous position of each falling character instead of refreshing the entire screen.
   - Using `GotoXY(x, y-1)` to move the cursor back and print a space (" ") before moving the raindrop down.
   - Clearing only the bottom row `(height - 1)` to prevent characters from stacking up at the bottom.

This method significantly improved the animation, making it look cleaner and more fluid, just like a proper Digital Rain effect.


### **Log 4: Replacing Magic Numbers & Theme Menu Implementation**

In this stage of development, two key enhancements were made to improve code clarity, maintainability, and functionality.

While functional, this approach made the code harder to understand and modify. To improve readability and future flexibility, this was replaced with clearly defined const char[] arrays:

  - **FULL_CHAR_SET[]** – Includes numbers, letters, and symbols.
  - **SYMBOL_CHAR_SET[]** – Contains symbols and special characters only, including currency signs.
    <img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/Log4b.png?raw=true" width="700" height="500">
  - Binary theme (0 and 1) is handled separately using a simple ternary operation.
    
These changes eliminate "magic numbers" and allow each theme to use a dedicated and understandable character set.

A theme selection menu was introduced to enhance user experience and provide visual variety. On program start, the following menu appears:

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/Log4a.png?raw=true" width="600" height="400">

Once a theme is selected, the screen is cleared and the corresponding rain animation begins.

#### **ESC Key Integration**
While the rain is running, users can press the ESC key to return to the main menu and choose a different theme. This was achieved using:

 - _kbhit() to check for keypresses.
 - _getch() to detect if the ESC key (27) is pressed
 - break to exit the rain loop and return to the menu
<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/Log4c.png?raw=true" width="600" height="400">

**Reference:** ChatGPT was used to help explore and implement the ESC key input logic. Available: https://chatgpt.com/


#### **Theme Descriptions**
 - Theme 1 – Multicolor Random:
Displays characters from FULL_CHAR_SET using a mix of colors like green, red, blue, and yellow.

 - Theme 2 – Matrix Symbols Only (Green):
Uses only characters from SYMBOL_CHAR_SET, rendered in bright green for an authentic Matrix effect.

 - Theme 3 – Binary (0 and 1):
Renders only '0' and '1' in plain white, simulating a binary code stream.


## **Algorithm**

### **Raindrop Falling Animation Logic:**

This algorithm continuously updates each raindrop’s position, simulates its fall, and handles its drawing and erasing.

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/AlgorithmsA.png?raw=true" width="600" height="400">

The first for loop, the loop goes through every RainDrop object in the rainDrops vector. Each RainDrop represents a vertical stream of falling characters. `rainDrop.get()` returns a std::vector<RainElement>, which contains the coordinates (x, y) and the character c that needs to be displayed. The elements variable holds these for the current drop, which could be multiple characters (depending on the drop's size).

In the second for loop, the loop is used to erase the previous character shown on the screen. `GotoXY(x, y-1)` moves the cursor to the previous Y position of the character. `std::cout << " "` prints a space to effectively clear the old character from the screen. It prevents the screen from looking cluttered or leaving behind character trails.

`rainDrop.fall()` moves the drop down by increasing its y value. After falling, `get()` is called again to retrieve the updated positions and characters that should now be rendered on screen.

The third for loop then draws the character in its new position. Then the cursor is moved to the updated coordinates using `GotoXY()`. 

### **Generating Rain Characters:**

<img src="https://github.com/dewanhasan/digital-rain-y4-cpp/blob/main/docs/assets/images/AlgorithmsB.png?raw=true" width="750" height="400">

This function returns a list of characters (RainElements) making up the vertical line of a raindrop. `getRandomChar()` is called to fetch the actual symbol, based on the currently selected theme. `(y - i + 25) % 25` allows wrapping characters vertically so drops loop at the bottom.


## **Problem Solving**

Throughout the development of the Digital Rain animation project, several technical problems were encountered and addressed. This section outlines the key problems and the solutions implemented to enhance performance, readability, and user experience.

### **Flickering Console Screen:**

**Problem:**
Initially, the screen was being cleared using system("cls") for every frame of the animation. This caused severe flickering due to the constant redrawing of the entire console screen.

**Solution:**
To eliminate flickering, the use of system("cls") inside the animation loop was removed and instead used a smarter approach:

- Only the previous position of each character was cleared `(GotoXY(x, y - 1)` followed by printing a space).
- Only the bottom row was cleared completely to prevent character stacking.
- This method provided smooth animation while keeping the screen visually clean.

### **Magic Numbers in Character Generation:**

**Problem:**
The initial version used raw ASCII values (magic numbers) such as static_cast<char>((rand() % (126 - 33 + 1)) + 33) to generate characters. This reduced code clarity and made it difficult to control which characters appeared.

**Solution:**
Magic numbers were replaced with descriptive and maintainable constants:

- `FULL_CHAR_SET[]` Contains a full set of alphanumeric and symbolic characters.
- `SYMBOL_CHAR_SET[]` Used exclusively in the Matrix theme, consisting only of symbols.

These sets made the character output more controllable and easily extendable.







## **Modern C++ Insight & Reflection**





