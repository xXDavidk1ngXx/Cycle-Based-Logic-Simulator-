# Cycle-Based Digital Logic Simulator

A professional-grade command-line tool for simulating digital circuits, implementing core concepts from Electronic Design Automation (EDA) software. Built with modern C++17, this simulator handles both combinational and sequential logic with cycle-accurate timing behavior.

## 🎯 Project Overview

This simulator reads netlist files describing digital circuits and accurately simulates signal propagation through logic gates over time. It demonstrates fundamental concepts used in industry-standard tools like ModelSim, VCS, and Cadence Simvision.

**Target Applications**: Chip Design CAD, Digital Verification, Hardware Simulation Tools

## ✨ Key Features

### Core Functionality
- **Combinational Logic**: AND, OR, NOT, XOR gates with multi-input support
- **Sequential Logic**: D Flip-Flops with clock-edge triggered behavior
- **Cycle-Based Simulation**: Event-driven evaluation with proper timing semantics
- **Signal States**: Support for HIGH (1), LOW (0), UNKNOWN (X), and HIGH-Z (Z) states

### Advanced Capabilities
- **Topological Sorting**: Efficient evaluation order calculation for combinational logic
- **Cycle Detection**: DFS-based algorithm to detect combinational loops
- **Truth Table Generation**: Exhaustive input combination testing
- **Signal Tracing**: Timestamped waveform output for debugging
- **Robust Error Handling**: Line-number accurate syntax error reporting

### Software Engineering
- **Modern C++17**: Smart pointers, RAII, const-correctness throughout
- **Object-Oriented Design**: Factory pattern, polymorphic component hierarchy
- **Memory Safety**: No raw pointers, automatic resource management
- **Extensibility**: Easy to add new gate types via base class interface
- **Professional Architecture**: Clean separation of parsing, simulation, and data layers

## 🏗️ Architecture
```
┌─────────────┐
│   Parser    │  Reads netlist files
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Circuit   │  Owns components and wires
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Simulator  │  Evaluates logic over time
└─────────────┘
```

**Component Hierarchy:**
```
Component (abstract base)
├── Combinational Gates
│   ├── AndGate
│   ├── OrGate
│   ├── NotGate
│   └── XorGate
└── Sequential Elements
    └── DFlipFlop
```

**Data Model:**
- `Wire` objects (shared via `std::shared_ptr`) connect components
- `Circuit` maintains ownership of all components and wires
- `Simulator` operates on circuit graph using topological ordering

## 🚀 Getting Started

### Prerequisites
- C++17 compatible compiler (GCC 7+, Clang 5+, MSVC 2017+)
- CMake 3.15 or higher
- Git

### Building from Source
```bash
# Clone the repository
git clone https://github.com/yourusername/logic-simulator.git
cd logic-simulator

# Build the project
mkdir build && cd build
cmake ..
make

# Run tests (optional)
make test
```

### Quick Start
```bash
# Generate truth table for a half adder
./logsim examples/half_adder.net --truth-table

# Simulate a counter for 16 clock cycles
./logsim examples/counter.net --cycles 16 --trace

# Set specific input values
./logsim examples/full_adder.net --input a=1 --input b=1 --input cin=0
```

## 📝 Netlist Format

The simulator uses a simple, human-readable netlist format:
```
# Comments start with #

# Declare circuit inputs and outputs
INPUTS a b cin
OUTPUTS sum cout

# Define components: <TYPE> <name> <inputs...> -> <outputs...>
XOR xor1 a b -> temp1
XOR xor2 temp1 cin -> sum
AND and1 a b -> temp2
AND and2 temp1 cin -> temp3
OR or1 temp2 temp3 -> cout
```

### Supported Components

| Component | Syntax | Description |
|-----------|--------|-------------|
| AND Gate | `AND name in1 in2 ... -> out` | Logical AND |
| OR Gate | `OR name in1 in2 ... -> out` | Logical OR |
| NOT Gate | `NOT name in -> out` | Logical inverter |
| XOR Gate | `XOR name in1 in2 ... -> out` | Exclusive OR |
| D Flip-Flop | `DFF name d clk reset -> q` | D-type flip-flop |

## 📚 Example Circuits

### Half Adder
```
INPUTS a b
OUTPUTS sum carry

XOR xor1 a b -> sum
AND and1 a b -> carry
```

### 2-bit Counter
```
INPUTS clk reset
OUTPUTS q0 q1

DFF dff0 d0 clk reset -> q0
DFF dff1 d1 clk reset -> q1

NOT not0 q0 -> d0
XOR xor0 q0 q1 -> d1
```

More examples available in the `examples/` directory.

## 🔧 Command-Line Interface
```
Usage: logsim <netlist.net> [options]

Options:
  --truth-table              Generate truth table for all input combinations
  --cycles N                 Run simulation for N clock cycles
  --input <name>=<0|1>       Set specific input value
  --trace                    Print signal trace over time
  --verbose                  Enable verbose output
  --check-cycles             Check for combinational loops
  --output-vcd <file.vcd>    Generate VCD waveform file
  --help                     Display this help message
```

## 🧪 Testing

The project includes comprehensive test circuits:

- **half_adder.net**: Basic combinational logic
- **full_adder.net**: Multi-level combinational circuits
- **sr_latch.net**: Sequential feedback (tests cycle handling)
- **counter.net**: Multi-bit sequential logic
- **alu_slice.net**: Complex arithmetic logic

Run all example circuits:
```bash
./run_tests.sh
```

## 🎓 Technical Highlights

### Graph Algorithms
- **Topological Sort (Kahn's Algorithm)**: Orders components for single-pass evaluation
- **Cycle Detection (DFS)**: Identifies combinational loops that prevent stabilization
- **Reachability Analysis**: Validates wire connectivity

### Simulation Algorithm
```cpp
1. Parse netlist → Build component graph
2. Topologically sort combinational components
3. For each clock cycle:
   a. Set input values
   b. Evaluate combinational logic until stable
   c. Check for oscillations (iteration limit)
   d. Trigger clock edge → Update flip-flops
   e. Record output states
```

### Memory Management
- **Smart Pointers**: `std::shared_ptr` for wires, `std::unique_ptr` where appropriate
- **No Raw Pointers**: Zero manual memory management
- **RAII Principle**: Automatic resource cleanup
- **Zero Memory Leaks**: Validated with Valgrind

## 🏆 Project Goals & Learning Outcomes

This project demonstrates:

✅ **Systems Programming**: Large-scale C++ application architecture  
✅ **Data Structures**: Graphs, topological sorting, hash tables  
✅ **Algorithms**: DFS, cycle detection, graph traversal  
✅ **Object-Oriented Design**: Inheritance, polymorphism, factory pattern  
✅ **Software Engineering**: Testing, documentation, build systems  
✅ **Domain Knowledge**: Digital logic, timing, hardware description languages  

**Perfect for**: ASIC Design, FPGA Engineering, EDA Software Development positions at companies like NVIDIA, AMD, Intel, AWS Annapurna Labs.

## 🛣️ Roadmap

### Current Version (v1.0)
- [x] Basic combinational gates
- [x] D Flip-Flops
- [x] Cycle detection
- [x] Truth table generation
- [x] CLI interface

### Future Enhancements
- [ ] VCD waveform export (GTKWave compatible)
- [ ] More sequential elements (SR, JK flip-flops, registers)
- [ ] Tri-state buffer support
- [ ] Hierarchical modules (subcircuits)
- [ ] Timing delays and propagation modeling
- [ ] VHDL/Verilog netlist import
- [ ] GUI with graphical circuit editor

## 📖 Documentation

Detailed documentation available in `/docs`:
- [Architecture Guide](docs/architecture.md) - System design and component interaction
- [Netlist Specification](docs/netlist_spec.md) - Complete format reference
- [API Reference](docs/api.md) - Component interface documentation
- [Algorithm Deep-Dive](docs/algorithms.md) - Simulation engine internals

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Areas for contribution:
- New gate types (NAND, NOR, XNOR, multiplexers)
- Performance optimizations
- Additional example circuits
- Test coverage improvements
- Documentation enhancements

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.

## 👨‍💻 Author

**Your Name**  
[LinkedIn](https://linkedin.com/in/yourprofile) | [GitHub](https://github.com/yourusername) | [Email](mailto:your.email@example.com)

*Developed as a portfolio project demonstrating software engineering skills for chip design CAD and digital verification roles.*

## 🙏 Acknowledgments

Inspired by industry-standard EDA tools:
- ModelSim (Mentor Graphics)
- VCS (Synopsys)
- Xcelium (Cadence)
- Icarus Verilog

Special thanks to the digital design community for resources on logic simulation algorithms.

## 📞 Contact

Questions or feedback? Open an issue or reach out directly!

---

**Note**: This is an educational project built to demonstrate proficiency in C++, algorithms, and digital design concepts. For production hardware verification, please use industry-standard commercial tools.
