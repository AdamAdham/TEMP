# Automation of Re-architecture of Software Test Automation Library using Generative AI
## **Abstract:**

- Test Automation Libraries for EDA applications are significantly large in size and classes complexity, as well as being built over several years. Additionally,  with the new trend of web based EDA application building a reliable test libraries that cover collaborative testing, visual testing, multi browser testing, UI performance, response time as well as product capacity and scalability bring more challenges to the automation. Server loads, Security Access, Debug views correlations and more are just few challenges that should be part of this test automation framework  
- This project will focus on creating complete automated testing framework, tool kit for web-based EDA application by understand existing automation system, utilize generative AI to understand the code, the correlation and re-architect the entire system to fulfill the product scalability needs as well as the regression associated with it.
- In this project we will study the best practices for Test Automation Code for Web Based Applications, and explore Generative AI and LLM based techniques to refactor the existing code and also explore how to migrate from existing technology, being Selenium, to a new automation technology which is Microsoft Playwright
# Notes
### EDA
Electronic Design Automation
## RAG 
(Retrieval-Augmented Generation)**:

## SAST
Static Application Security Testing
	 
- **Benefits**:
    
    - **Early Detection**: SAST tools can identify vulnerabilities early in the development process, often during coding or just after code changes, allowing for quicker remediation.
    - **Comprehensive Coverage**: They analyze all parts of the codebase, including code that may not be executed during typical testing, ensuring a thorough examination of potential security issues.
- **Limitations**:
    
    - **False Positives**: SAST tools might report vulnerabilities that are not actual security issues, requiring manual verification by developers or security experts.
    - **Limited Context**: Since SAST tools don’t execute the code, they might miss vulnerabilities that only manifest during runtime or in specific execution contexts.

## **Language Agnostic Embeddings**:
Language agnostic embeddings refer to methods that allow code written in different programming languages to be represented and compared in a similar way. This means that the same underlying concepts or logic in code can be understood and compared across different languages, such as Python, Java, or C++.

**Agnostic:** means not limited or specific to one particular language, tool, or technology.
## Questions

Will we get provided with previous tests w for FPGAs also? (heya dih el dr eman alet 3aleiha?)

"re-architect the entire system to fulfill the product scalability needs as well as the regression associated with it."?


<br><br>
<br><br>
<br><br>

# FPGA Synthesis Needs from RTL specification ML_AI Based Prediction

Abstract 

In this project, it’s expected to leverage machine learning or AI technique to predict the results of synthesis tools (in terms of type and count instantiated cells, attributes of cells, consumed area, power, ..).

#### Project Objectives and milestones

- Learn different FPGA synthesis flows and devices
- Learn how synthesized cells are inferred according to the RTL templates
- Extract Cell Features from existing Regressions like (tiling , merging , packing ,…)
- Understand brand-new machine learning for
- Classification : Given a set of features to be inferred in the netlist → return a list of test cases that most probably will infer these features (even for a new device, family)
- Autotest creation : Given a set of features to be inferred in the netlist → suggest new RTLs if no RTLs cover them in the regression
- Create Visualization page for the characterization part

# Notes

### HDL vs RTL

HDL (Hardware description Language) is the type of language used, Verilog/VHDL versus a non-HDL javascript.

RTL (Register-transfer level) is a level of abstraction that you are writing in. The three levels I refer to are Behavioural, RTL, Gate-level.

**Highest level --> Lowest level**
Behavioural --> RTL --> Gate-level

**Behavioral** has the highest layer of abstraction which describes the overall behavior and is often not synthesizable, but is useful for verification.

**RTL** describes the hardware you want by implying logic. defining flip-flops, latches and how data is transfered between them. This is synthesizable, synthesis may alter/optimize the logic used but not behavior. Switching muxes for gates etc some times inverting signals to better optimize the design.

Verilog RTL implying a flip-flop:
```
logic a;              //logic is SystemVerilog, could be a 'reg'
logic k;              // Driven by RTL not shown
always @(posedge clk or negede rst_n) begin
  if (~rst_n) begin
    a <= 'b0 ;
  end
  else begin
    a <= k ;
  end
end
```

Combinatorial Bitwise operators :

```
logic [1:0] n;
logic [1:0] m;
logic [1:0] result;

assign result = n & m ;
```


**Gate level** is a design using the base logic gates (NAND, NOR, AND, OR, MUX, FLIP-FLOP). It does not need to be synthesized or is the output from synthesis. This has the lowest level of abstraction. it is the logic gates that you will use on the chip, but it lacks positional information.

Gate level Verilog (same function as above):

```
wire a;
wire k;
DFFRX1 dffrx1_i0 (
  .Q (a),   //Output
  .QN( ),   //Inverted output not used
  .D (k),   //Input
  .CK(clk), //Clk
  .RN(rst_n)// Active Low Async Reset
);
```

Combinatorial

```
wire [1:0] n;
wire [1:0] m;
wire [1:0] result;

AND2X1 and2x1_i0 (
  .Y( result[0]),
  .A( n[0]     ),
  .B( m[0]     )
);
AND2X1 and2x1_i1 (
  .Y( result[1]),
  .A( n[1]     ),
  .B( m[1]     )
);
```

## **Synthesis**
**Synthesis** is the process of converting a high-level hardware description written in a Hardware Description Language (HDL) like Verilog or VHDL into a lower-level representation that can be physically implemented on hardware, such as an FPGA (Field-Programmable Gate Array) or an ASIC (Application-Specific Integrated Circuit).


### What is Synthesis?

**Synthesis** is the process of converting a high-level hardware description written in a Hardware Description Language (HDL) like Verilog or VHDL into a lower-level representation that can be physically implemented on hardware, such as an FPGA (Field-Programmable Gate Array) or an ASIC (Application-Specific Integrated Circuit).

### The Synthesis Process

The synthesis process typically involves several key steps:

1. **Parsing and Elaboration:**
    
    - **Parsing:** The synthesis tool first parses the HDL code to check for syntax errors and understand the structure of the design.
    - **Elaboration:** The tool then elaborates the design, resolving any high-level constructs, such as parameters and generates statements, to create a complete and fully defined netlist.
2. **Translation to RTL Netlist:**
    
    - The HDL code, which describes the design at a high level (e.g., behavioral or RTL), is translated into an RTL netlist. A netlist is a list of logical components (like registers, adders, multiplexers, etc.) and the connections between them.
    - This RTL netlist describes how data flows through the design and how operations are performed, usually using basic digital components.
3. **Optimization:**
    
    - The synthesis tool optimizes the RTL netlist for various metrics such as area, speed, and power consumption.
    - **Logic Optimization:** Redundant logic may be removed, and equivalent transformations may be applied to make the circuit more efficient.
    - **Technology Mapping:** The optimized logic is mapped to the specific library of standard cells (for ASIC) or configurable logic blocks (for FPGA).
4. **Gate-Level Netlist Generation:**
    
    - The synthesis tool generates a gate-level netlist, which represents the design using specific logic gates and flip-flops available in the target technology library.
    - This netlist is a lower-level representation that is directly related to the physical implementation.
5. **Timing Analysis:**
    
    - The tool performs static timing analysis to ensure that the synthesized design meets the required timing constraints, such as clock speed.
    - If timing issues are detected, further optimizations or adjustments may be necessary.
6. **Placement and Routing (For FPGA Synthesis):**
    
    - After synthesis, the design undergoes placement and routing, where the synthesized logic is mapped onto the actual hardware resources of the FPGA.
    - The tool determines where each logic block should be placed on the FPGA and how the connections (wires) between them should be routed.

### Why is Synthesis Done?

Synthesis is a crucial step in the digital design flow because it bridges the gap between high-level design and physical implementation. The reasons for performing synthesis include:

1. **Implementation on Hardware:**
    
    - To transform a high-level description into a format that can be directly implemented on physical hardware (FPGA or ASIC).
2. **Optimization:**
    
    - To optimize the design for performance, area, and power consumption. The synthesis tool can make decisions to improve these aspects based on the target technology.
3. **Verification:**
    
    - To verify that the high-level design meets the desired specifications and constraints once synthesized into hardware.
4. **Automation:**
    
    - Synthesis automates the process of converting a design from an abstract description to a concrete implementation, saving time and reducing human error.

### Example of Synthesis:

Let’s say you write a simple Verilog code for a 4-bit adder, as we discussed earlier:

```verilog
module adder_rtl (
    input wire [3:0] a,
    input wire [3:0] b,
    input wire cin,
    output reg [3:0] sum,
    output reg cout
);

always @(posedge clk or posedge reset) begin
    if (reset) begin
        sum <= 4'b0000;
        cout <= 0;
    end else begin
        {cout, sum} <= a + b + cin;
    end
end

endmodule
```

When you run this code through a synthesis tool:

- **Parsing/Elaboration:** The tool checks the syntax and interprets the design, resolving parameters, and macros.
- **Translation to RTL Netlist:** The tool converts this code into an RTL netlist, representing the adder in terms of basic digital operations.
- **Optimization:** The RTL netlist is optimized to use the fewest possible resources or to meet specific performance goals.
- **Gate-Level Netlist Generation:** The tool maps the RTL to a gate-level netlist using actual logic gates and flip-flops from a technology library.
- **Timing Analysis:** The tool ensures that the design will meet the required timing constraints.
- **Placement and Routing (FPGA):** Finally, if you’re targeting an FPGA, the logic is placed and routed on the FPGA’s resources.

### **How Synthesis is Performed:**

1. **Select the Synthesis Tool:**
    
    - Tools like Xilinx Vivado, Intel Quartus, Synopsys Design Compiler, or Cadence Encounter are used depending on whether you're targeting FPGAs or ASICs.
2. **Configure the Tool:**
    
    - Set the target device (FPGA or ASIC), specify timing constraints, and configure optimization settings.
3. **Run Synthesis:**
    
    - The tool processes the HDL code and generates the synthesized netlist. This netlist can then be used for simulation, verification, or direct implementation on hardware.
4. **Analyze Results:**
    
    - Review the timing reports, area usage, and other metrics provided by the synthesis tool to ensure the design meets the required specifications.

### **Conclusion:**

Synthesis is a fundamental step in digital design, converting a high-level description into a form that can be physically realized on hardware. It involves optimization, timing analysis, and technology mapping, ensuring the final design is efficient and meets the desired performance criteria.


### **High-Level Constructs**

1. **Parameters:**
    
    - **Definition:** Parameters are used in HDL to define constants that can be used throughout the design. They allow you to create flexible and reusable modules by specifying values that can be changed without modifying the core module code.
    - **Example:** In Verilog, parameters might be used to define the width of data buses or the number of bits in a counter.
    
    verilog
    
    Copy code
    
    `module adder #(     parameter WIDTH = 4  // Parameter defining the width of the adder ) (     input wire [WIDTH-1:0] a,     input wire [WIDTH-1:0] b,     output wire [WIDTH-1:0] sum ); // Adder implementation here endmodule`
    
    During elaboration, the synthesis tool replaces these parameters with specific values. For instance, if `WIDTH` is set to 4, the tool will instantiate the adder module with a 4-bit width for `a`, `b`, and `sum`.
    
2. **Generate Statements:**
    
    - **Definition:** Generate statements in HDL are used to create multiple instances of hardware or to conditionally include parts of the design based on parameters or other conditions. They are particularly useful for creating repetitive structures or conditional logic.
    - **Example:** In Verilog, generate statements might be used to instantiate a set of flip-flops or to include specific blocks of code based on a parameter value.
    
    verilog
    
    Copy code
    
    `module counter #(     parameter WIDTH = 4  // Parameter defining the width of the counter ) (     input wire clk,     input wire reset,     output reg [WIDTH-1:0] count );  genvar i; generate     for (i = 0; i < WIDTH; i = i + 1) begin : gen_counter         always @(posedge clk or posedge reset) begin             if (reset)                 count[i] <= 0;             else                 count[i] <= count[i] + 1;         end     end endgenerate endmodule`
    
    In this example, the `generate` block creates multiple instances of the flip-flops based on the `WIDTH` parameter. During elaboration, the synthesis tool will expand this block into the actual hardware instances required for the design.
    

### **Creating a Complete and Fully Defined Netlist**

During elaboration, the synthesis tool:

1. **Resolves Parameters:**
    
    - It replaces parameterized values with their actual values. If parameters are used to define the size or quantity of components, the tool will instantiate these components with the correct sizes or quantities.
2. **Expands Generate Statements:**
    
    - It processes generate statements to create the actual instances of hardware as specified. This involves expanding loops or conditionals to produce the final, concrete hardware structure.
3. **Completes the Design:**
    
    - It resolves any other high-level constructs like module instantiations and hierarchical references to produce a comprehensive netlist. The netlist represents the design in terms of basic digital components (e.g., gates, flip-flops) and their connections, ready for further optimization and synthesis.

## **NETLIST**

Here’s the Verilog code we’ll be working with:

``` verilog
module two_bit_adder (
    input wire [1:0] a,    // 2-bit input a
    input wire [1:0] b,    // 2-bit input b
    input wire cin,        // Carry-in input
    output wire [1:0] sum, // 2-bit sum output
    output wire cout       // Carry-out output
);

assign {cout, sum} = a + b + cin; // Addition operation with carry

endmodule

```

### Translation to RTL Netlist

#### 1. Component Declaration

```verilog
module two_bit_adder (
    input wire [1:0] a,    // 2-bit input a
    input wire [1:0] b,    // 2-bit input b
    input wire cin,        // Carry-in input
    output wire [1:0] sum, // 2-bit sum output
    output wire cout       // Carry-out output
);

```

**Explanation:**

- **Module Declaration:** Defines a new module named `two_bit_adder`.
- **Ports:** `a` and `b` are 2-bit input wires, `cin` is a single-bit carry-in input, `sum` is a 2-bit output, and `cout` is a single-bit carry-out output.

**RTL Netlist Equivalent:**

- **Component Instantiation:** The RTL netlist will instantiate logical components to perform the addition operation, such as full adders.

#### **2. Addition Operation**

**Verilog Code:**

```verilog
assign {cout, sum} = a + b + cin; // Addition operation with carry
```

**Explanation:**

- **Assignment Statement:** This line performs a 2-bit addition of `a` and `b`, and includes the carry-in `cin`. It then assigns the result to the `sum` and `cout` outputs. The `cout` (carry-out) will be the result of the most significant bit of the addition, and `sum` will be the 2-bit result of the addition.

**RTL Netlist Equivalent:**

- **Full Adder Instances:** The RTL netlist will use full adders to perform this addition. Each full adder handles the addition of two bits and a carry-in, producing a sum and carry-out.

#### **RTL Netlist Creation**

Here’s how we might translate the above Verilog code into an RTL netlist:

**RTL Netlist:**

``` verilog
// NOT VERILOG JUST FOR HIGHLIGHTING
// Define components
FULL_ADDER  U1 (.A(a[0]), .B(b[0]), .Cin(cin), .Sum(sum[0]), .Cout(carry1)); # Full adder for LSB
FULL_ADDER  U2 (.A(a[1]), .B(b[1]), .Cin(carry1), .Sum(sum[1]), .Cout(cout)); # Full adder for MSB

# Connections (Nets)
N1 carry1;     # Net connecting the carry-out from the LSB adder to the MSB adder

# Component Connections
U1.A = a[0];
U1.B = b[0];
U1.Cin = cin;
U1.Sum = sum[0];
U1.Cout = carry1;

U2.A = a[1];
U2.B = b[1];
U2.Cin = carry1;
U2.Sum = sum[1];
U2.Cout = cout;

```

**Explanation:**

1. **Component Definition:**
    
    - **FULL_ADDER:** Represents a full adder component used for the addition. There are two instances: `U1` for the least significant bit (LSB) and `U2` for the most significant bit (MSB).
2. **Instance `U1`:**
    
    - **Inputs:** `A` and `B` are connected to `a[0]` and `b[0]`, respectively. `Cin` is connected to `cin` (carry-in).
    - **Outputs:** `Sum` is connected to `sum[0]` (the result of the LSB addition), and `Cout` is connected to `carry1`.
3. **Instance `U2`:**
    
    - **Inputs:** `A` and `B` are connected to `a[1]` and `b[1]`, respectively. `Cin` is connected to `carry1` (carry from the LSB adder).
    - **Outputs:** `Sum` is connected to `sum[1]` (the result of the MSB addition), and `Cout` is connected to `cout`.
4. **Nets:**
    
    - **carry1:** This net connects the carry-out from the LSB adder (`U1`) to the carry-in of the MSB adder (`U2`).

### **Step-by-Step Thought Process:**

1. **Understand the Operation:**
    
    - The Verilog code performs a simple 2-bit addition with a carry-in. This operation needs to be broken down into basic components in the RTL netlist.
2. **Identify Components:**
    
    - For addition, the basic component is a full adder. Each full adder handles the addition of two bits and a carry-in.
3. **Instantiate Components:**
    
    - Create instances of the full adder for each bit of the input. The LSB (least significant bit) adder handles the addition of `a[0]` and `b[0]`, while the MSB (most significant bit) adder handles the addition of `a[1]` and `b[1]`, along with the carry from the LSB adder.
4. **Connect Components:**
    
    - Ensure that the carry-out from the LSB adder is connected to the carry-in of the MSB adder. This ensures that the carry is correctly propagated through the addition process.
5. **Define Outputs:**
    
    - Connect the sum outputs of each adder to the final `sum` output, and the final carry-out to `cout`.

Fulladder here is a predefined function
