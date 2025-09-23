# Introduction to Hierarchical and Flattern synthesis, exploring ``.lib`` file

In this we are going to explore how ``.lib`` file read and the concept of Hierarchical and flattern synthesis with their advantages and disadvantages.

---

## 1. Exploring ``.lib`` file

* **Install Neovim Text Editor**

    ```bash
    sudo apt install neovim -qt
    ```
* **Open sky130_fd_sc_hd__tt_025C_1v80.lib**

    ```bash
    gvim sky130_fd_sc_hd__tt_025C_1v80.lib 
    ```

    * If you don't like the syntax color press ``Shift`` + ``:``
    * Then write 
        ``bash
        syn off
        ``
---

## 2. Introduction to Hierarchical Synthesis

Hierarchical Synthesis is also known as **Divide and Conqure** Design Methedology.
In this type of synthesis a large circuit module is divided into smaller submodules which can work as seperate modules and their ouputs can be connected in such a way that our large circuit recreated.
This means the submodules are synthesized individually and then integrated as single large module.

### I. Advantages
* **Complexity management**
    - The circuit modules can be so large which can't be handled by a single individual.
    - For minimizing the complexity we divide module into submodules and work on them properly.

* **Synthesis Time Reduce**
    - Synthesis of smaller submidules takes small amount of time compare to larger modules.

### II. Disadvatnages
* **Complexity in Management**

    - In hierarchical projects, managing the design flow is real task.
    - We have to handle different version of submodules.
    - Take care of constraints and timing info while moving the across different levels.

* **Require more skills**
    - The designers must have good knowledge of full design flow.
    - They must know to divide properly and give accurete constrains.

### III. Lab on Hierarchical Synthesis

* **Step1 - Open Yosys**
    ```bash
    yosys
    ```

* **Step2 - Load standerd cell library**
    ```bash
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  //use path to locate sky130_fd_sc_hd__tt_025C_1v80.lib file
    ```
* **Step3 - Read verilog design**
    ```bash
    read_verilog multiple_modules.v      //Loads your HDL design 
    ```
* **Step4 - Define the top Module**
    ```bash
    synth -top multiple_modules  //Synthesize your RTL into generic gate
    ```
* **Step5 - Technplogy mapping with abc**
    ```bash
    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    ```
* **Step6 - View the Schematic**
    ```bash
    show multiple_modules
    ```
![Hierarchical](Images/m_m%20hier.png)

---

## 3. Introduction to Flatten Synthesis

Flatten Synthesis is also known as **Flat Synthesis**.
Unlike Hierarchical Synthesis, we synthesize the entire large module of numerus logic gates in one.
The synthesis tool takes all the individual sub-modules and their interconnections and processes them simultaneously, creating a single, comprehensive netlist.

## I. Advantages

* **Simplified Design Flow**
    - No need to handle complex constraints between modules or timing of each block. 
    - All constraints are applied only at the top level, making the process easier.

* **Best for Small to Medium Designs** 
    - For circuits that are not excessively large, flat synthesis can often produce better Quality of Results in terms of area and timing because the tool is not constrained by module boundaries.

## II. Disadvatages

* **Difficult Debugging**

    - As the netlist becomes flat (only gates and wires). This makes debugging hard because original blocks can’t be seen, and tracing signals back to HDL code is difficult.

* **Poor Scalability**
    - The memory and processing power is so much required to handle millions of gate at once.
    - Any mistake in processing cause tools to crash and thats why industry not use Flatten Synthesis for mordern chip design.

### III. Lab on Flatten Synthesis

* **Step1 - Open Yosys**
    ```bash
    yosys
    ```

* **Step2 - Load standerd cell library**
    ```bash
    read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  //use path to locate sky130_fd_sc_hd__tt_025C_1v80.lib file
    ```
* **Step3 - Read verilog design**
    ```bash
    read_verilog multiple_modules.v      //Loads your HDL design 
    ```
* **Step4 - Define the top Module**
    ```bash
    synth -top multiple_modules  //Synthesize your RTL into generic gate
    ```
* **Step5 - Technplogy mapping with abc**
    ```bash
    abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
    ```
* **Step6 - Do Flatten Synthesis**
    ```bash
    flatten
    ```
* **Step7 - View the Schematic**
    ```bash
    show multiple_modules
    ```
![Hierarchical](Images/m_m%20flat.png)

---


