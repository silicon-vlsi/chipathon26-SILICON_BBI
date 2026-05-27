# Getting started with TLV

This was created by Git CoPilot from https://github.com/rweda/Makerchip-public

**Short answer:** for your goal, the best path in this repo is:

1. **Makerchip Tutorial** → learn the IDE and workflow  
2. **basic TL-Verilog tutorials** in `tutorial/tlv/` → learn syntax, staging, alignment, and flow  
3. **RISC-V Course+Single-Cycle** → best beginner path for architecture + TL-Verilog together  
4. **RISC-V Videos+Workshop / MYTH materials** → reinforce with guided labs  
5. **mini CPU examples** in `tutorial/tlv/` → study small CPUs before full RISC-V  
6. **docs/papers** → only after you’ve built intuition

Also, code search results are capped, so my findings may be incomplete. To browse more matches in GitHub UI, use the repository code search for terms like `TL-Verilog`, `RISC-V`, `tutorial`, and `cpu` in `rweda/Makerchip-public`.

## Best materials to start with

### 1) `README.md`
Best starting index for the whole repo.

````markdown name=README.md url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/README.md#L42-L67
## Repository Content Overview

This repository contains the following directories (alphabetically):

- `courses/`: PDF slides for various TL-Verilog courses. Most are focused on RISC-V CPU design. Some courses have accompanying content, such as a page of videos that can be found in the `pane-blade` folder.
- `docs/`: Documentation and specifications including those for TL-X, M5, and Visual Debug.
- `pane-blade/`: Each file provides the contents for a pane that can be opened within the Makerchip IDE.
...
### Courses

In addition to the tutorials, there are courses, summarized in <a href="pane-blade/Courses.blade" target="_blank">this page</a>. There are several courses based on RISC-V.
````
Why it matters:
- gives you the repo map
- explicitly points to **tutorials**, **courses**, and **RISC-V material**
- confirms this repo is meant as a learning hub

**Level:** absolute beginner

---

### 2) `pane-blade/Makerchip Tutorial.blade`
Best first hands-on onboarding to the Makerchip environment.

```blade name=pane-blade/Makerchip Tutorial.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/Makerchip%20Tutorial.blade#L33-L104
p  
  | If you already know Verilog, SystemVerilog, or TL-Verilog, you’ll be able to use #{appConfig.product} immediately.
  | The environment is intuitive, and this tutorial will step you through it. The remaining tutorials
  | provide an introduction to TL-Verilog and it's integration with #{appConfig.product}. Knowledge
  | of Verilog/SystemVerilog is not required.
...
ul
  li
    b Editor
    | : where you edit your Verilog/SystemVerilog/TL-Verilog code
  li
    b Log
    | : compiler and simulator output log
  li For debugging the design and simulations:
```

What it covers:
- Makerchip IDE basics
- editor, log, Nav TLV, diagram, waveform
- compile/simulate/debug workflow

Why relevant:
- if you’re new to TL-Verilog, the tool experience matters a lot
- you’ll learn how to inspect stages/signals visually before worrying about architecture

**Level:** absolute beginner

---

### 3) `tutorial/tlv/combinatorial_tutorial_5.tlv`
A tiny TL-Verilog example for expression syntax and basic signal usage.

```tl-verilog name=tutorial/tlv/combinatorial_tutorial_5.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/combinatorial_tutorial_5.tlv#L1-L19
// Stimulus
$op[0:0] = *cyc_cnt[0];
$num1[7:0] = *cyc_cnt[7:0];
$num2[7:0] = *cyc_cnt[8:1];

// Add when $op, else subtract.
$rslt[7:0] = ($op == 1) ? $num1 + $num2 :
                          $num1 - $num2;
```

What it covers:
- plain signal declarations/assignments
- combinational logic
- simple checking

Why relevant:
- fastest way to get used to TL-Verilog’s surface syntax
- low cognitive load before pipelines

**Level:** absolute beginner

---

### 4) `tutorial/tlv/pipeline_tutorial.tlv`
Best first look at TL-Verilog pipeline staging.

```tl-verilog name=tutorial/tlv/pipeline_tutorial.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/pipeline_tutorial.tlv#L1-L28
|calc
   @1
      $aa_sq[7:0] = $aa[3:0] ** 2;
      $bb_sq[7:0] = $bb[3:0] ** 2;
   // [+] @2
      $cc_sq[8:0] = $aa_sq + $bb_sq;
   // [+] @3
      $cc[4:0] = sqrt($cc_sq);
```

What it covers:
- pipeline notation `|pipe`
- stages `@1`, `@2`, `@3`
- moving from combinational thinking to staged design

Why relevant:
- TL-Verilog becomes powerful when you understand timing abstraction
- CPU work will depend heavily on this mental model

**Level:** beginner

---

### 5) `tutorial/tlv/alignment_tutorial.tlv`
Important for understanding valid/skip/alignment behavior.

```tl-verilog name=tutorial/tlv/alignment_tutorial.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/alignment_tutorial.tlv#L1-L45
|calc
   ?$valid
      @1
         $aa_sq[31:0] = $aa * $aa;
         $bb_sq[31:0] = $bb * $bb;
      @2
         $cc_sq[31:0] = $aa_sq + $bb_sq;
      @3
         $cc[31:0] = sqrt($cc_sq);
```

What it covers:
- valid transactions
- stage alignment
- reasoning about values across time

Why relevant:
- this is directly useful for pipelined CPU reasoning
- helps later with hazards and staged datapaths

**Level:** beginner

---

### 6) `tutorial/tlv/flow_tutorial.tlv` and `flow_example.tlv`
Useful once you want to understand tool flow and libraries.

```tl-verilog name=tutorial/tlv/flow_tutorial.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/flow_tutorial.tlv#L26-L39
m4_include_url(['https:/']['/raw.githubusercontent.com/stevehoover/tlv_flow_lib/5a8c0387be80b2deccfcd1506299b36049e0663e/fundamentals_lib.tlv'])
m4_include_url(['https:/']['/raw.githubusercontent.com/stevehoover/tlv_flow_lib/5a8c0387be80b2deccfcd1506299b36049e0663e/pipeflow_lib.tlv'])
m4_makerchip_module()
```

What it covers:
- M4 preprocessing
- library inclusion
- TL-Verilog flow concepts

Why relevant:
- helps you understand “what is happening under the hood”
- useful after basic syntax, not before

**Level:** beginner to intermediate

---

## Best architecture + RISC-V learning material

### 7) `pane-blade/Courses.blade`
This is the map of the course offerings.

```blade name=pane-blade/Courses.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/Courses.blade#L39-L59
h2
  | RISC-V CPU Design Courses
...
p
  | Computer Architecture is traditionally learned in a university setting as a
  | lecture-and-textbook course with a lab component.
...
p
  | they all start with logic gates and guide you to
  | building a simple RISC-V-based CPU.
```

What it covers:
- overview of available RISC-V courses
- explains that the courses begin from logic gates and build upward

Why relevant:
- confirms you do **not** need to already know architecture
- helps you choose the right course format

**Level:** absolute beginner

---

### 8) `pane-blade/RISC-V Course+Single-Cycle.blade`
This looks like the single best match for you.

```blade name=pane-blade/RISC-V Course+Single-Cycle.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/RISC-V%20Course%2BSingle-Cycle.blade#L1-L21
h1
  | Building a RISC-V CPU Core
...
p
  | This mini-workshop is a crash course in digital logic design and basic CPU microarchitecture.
  | Using the Makerchip online integrated development environment (IDE), you’ll implement everything
  | from logic gates to a simple, but complete, RISC-V CPU core.
```

And later:

```blade name=pane-blade/RISC-V Course+Single-Cycle.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/RISC-V%20Course%2BSingle-Cycle.blade#L60-L95
li
  | This course starts very basic with logic gates.
li
  | You may already have experience with digital logic, and this is fine. Don’t skip ahead.
li
  | you’ll zip right through them, to get very quickly to TL-Verilog, CPU architecture, and RISC-V.
```

What it covers:
- digital logic foundations
- TL-Verilog in context
- CPU microarchitecture
- a simple RISC-V CPU core
- Makerchip workflow
- debug and visualization

Why relevant:
- exactly matches your goal: beginner + RISC-V + TL-Verilog + quick ramp-up
- “Single-Cycle” also means simpler architectural entry than a deeply pipelined design

**Level:** absolute beginner to beginner

---

### 9) `pane-blade/RISC-V Videos+Workshop.blade`
Good if you prefer video-guided learning and quizzes.

```blade name=pane-blade/RISC-V%20Videos+Workshop.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/RISC-V%20Videos%2BWorkshop.blade#L34-L66
h3
  | Concepts Covered
p
  | The course covers:
  ul
    li
      | logic gates
    li
      | combinational logic
    li
      | sequential logic
    li
      | pipelined logic
    li
      | clock gating
    li
      | RISC-V CPU core microarchitecture
    li
      | memories and registers
    li
      | waterfall diagrams
    li
      | control and data hazards
    li
      | register bypass/forwarding
```

What it covers:
- architecture topics beyond single-cycle basics
- hazards, forwarding, pipelining
- quizzes and lesson progression

Why relevant:
- great second pass after the single-cycle material
- useful when you’re ready to move from “CPU works” to “CPU pipeline works”

**Level:** beginner

---

### 10) `pane-blade/MYTH Videos+Workshop.blade`
Another strong guided path with labs and starter code.

```blade name=pane-blade/MYTH%20Videos+Workshop.blade url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/pane-blade/MYTH%20Videos%2BWorkshop.blade#L12-L39
p
  | Open
  | Calculator Starting-point Code
...
p
  | Save your calculator code outside of Makerchip, then click to load the EDITOR with the
button
  | RISC-V Starting-point Code
```

What it covers:
- workshop flow
- starter code
- lab solutions
- progression from calculator to RISC-V

Why relevant:
- labs are excellent for beginners
- gives you a scaffold instead of a blank editor

**Level:** beginner

---

## Best code examples to study before building your own core

### 11) `tutorial/tlv/mini_cpu_1_cyc.tlv`
One of the most valuable files for you.

```tl-verilog name=tutorial/tlv/mini_cpu_1_cyc.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/mini_cpu_1_cyc.tlv#L28-L91
// A dirt-simple CPU for educational purposes.

// What's interesting about this CPU?
//   o It's super small.
//   o It's easy to play with and learn from.
...
// Machine Arch:
//   o Single stage "pipeline".
//   o 8 registers.
//   o A word is 12 bits wide.
```

What it covers:
- a deliberately tiny educational CPU
- ISA design ideas
- register file / PC / load-store / branch concepts in a simplified setting

Why relevant:
- easier to understand than jumping directly into full RISC-V
- ideal bridge from TL-Verilog syntax to CPU organization

**Level:** beginner

---

### 12) `tutorial/tlv/mini_cpu_parameterized.tlv`
Study this after `mini_cpu_1_cyc.tlv`.

```tl-verilog name=tutorial/tlv/mini_cpu_parameterized.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/mini_cpu_parameterized.tlv#L146-L183
// Adjust the parameters below to define the pipeline depth and staging.
m4_define(M4_PC_MUX_STAGE, 0)
m4_define(M4_FETCH_STAGE, 0)
m4_define(M4_DECODE_STAGE, 1)
m4_define(M4_REG_RD_STAGE, 1)
m4_define(M4_EXECUTE_STAGE, 2)
...
```

What it covers:
- parameterized staging
- pipeline depth choices
- CPU evolution from simpler to more advanced timing organization

Why relevant:
- excellent introduction to how TL-Verilog helps restructure pipelines
- useful before attempting a pipelined RISC-V

**Level:** beginner to intermediate

---

### 13) `tutorial/tlv/makerchip_tutorial.tlv`
Not the best first CPU to understand, but a good interactive CPU specimen.

```tl-verilog name=tutorial/tlv/makerchip_tutorial.tlv url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/tutorial/tlv/makerchip_tutorial.tlv#L26-L75
// Fetch pipe fetches an insruction.
|fetch
   @1
   @2
// A pipeline for execution, beginning from fetch response from memory.
|exe
   @2
   @3
   @4
   @5
   @6
   @7
```

What it covers:
- multi-stage pipeline structure
- fetch / execute / response partitioning
- a more realistic CPU-like organization

Why relevant:
- useful once you already understand stages and a simple CPU
- good for seeing nontrivial TL-Verilog pipeline structure

**Level:** intermediate for your current background

---

## Docs and papers worth reading later

### 14) `docs/` papers referenced by the README
The README points to these as core background:

```markdown name=README.md url=https://github.com/rweda/Makerchip-public/blob/36540e58fbec538e79bed0fd5c3010bed470aad5/README.md#L52-L59
- docs/ICCD2017Paper.pdf
- Top-Down Transaction-Level Design with TL-Verilog
- Formally Verifying WARP-V, an Open-Source TL-Verilog RISC-V Core Generator
```

What they cover:
- TL-Verilog design philosophy
- timing abstraction
- a real TL-Verilog RISC-V core generator

Why relevant:
- these give deeper conceptual grounding
- WARP-V is especially motivating once you know basic CPU structure

Why not first:
- papers are denser than tutorials/workshops
- better as reinforcement, not first exposure

**Level:** intermediate

---

## My recommended study plan for you

## Phase 1: Learn the environment and TL-Verilog basics
Spend 1–2 days here.

1. `README.md`
2. `pane-blade/Makerchip Tutorial.blade`
3. `tutorial/tlv/combinatorial_tutorial_5.tlv`
4. `tutorial/tlv/pipeline_tutorial.tlv`
5. `tutorial/tlv/alignment_tutorial.tlv`

Goal:
- understand Makerchip workflow
- be comfortable with signals, stages, and debugging
- stop being intimidated by TL-Verilog syntax

---

## Phase 2: Learn CPU ideas with the smallest possible examples
Spend 2–3 days here.

1. `tutorial/tlv/mini_cpu_1_cyc.tlv`
2. `tutorial/tlv/mini_cpu_parameterized.tlv`
3. `tutorial/tlv/makerchip_tutorial.tlv` (skim, don’t obsess)

Goal:
- understand PC, instruction execution, decode, register read/write, branch/load/store
- see how TL-Verilog expresses a CPU datapath over stages

---

## Phase 3: Do the full beginner RISC-V path
Spend 1–2 weeks here.

1. `pane-blade/Courses.blade`
2. `pane-blade/RISC-V Course+Single-Cycle.blade`

This is the **main course** I’d recommend for you.

Why:
- it starts from logic gates
- it reaches a simple complete RISC-V core
- it combines architecture and TL-Verilog rather than treating them separately

---

## Phase 4: Reinforce with workshop/video materials
After or alongside Phase 3.

1. `pane-blade/RISC-V Videos+Workshop.blade`
2. `pane-blade/MYTH Videos+Workshop.blade`

Goal:
- get repetition through another teaching style
- use labs and starter code
- strengthen concepts like pipelining, hazards, forwarding

---

## Phase 5: Read deeper material only after you’ve built something
1. `docs/ICCD2017Paper.pdf`
2. TL-Verilog papers linked in `README.md`
3. WARP-V references in `README.md`

Goal:
- understand why TL-Verilog is powerful
- connect your simple core to more advanced RISC-V implementations

---

## If you want the fastest possible path
If your only goal is **“build a simple RISC-V core quickly”**, do this:

1. `pane-blade/Makerchip Tutorial.blade`
2. `tutorial/tlv/pipeline_tutorial.tlv`
3. `tutorial/tlv/mini_cpu_1_cyc.tlv`
4. `pane-blade/RISC-V Course+Single-Cycle.blade`
5. `pane-blade/RISC-V Videos+Workshop.blade` only for topics you struggle with

That’s the most efficient path I found.

## Practical advice for an EE undergrad
Don’t try to master all of these at once. Learn in this order:
- **digital logic**
- **TL-Verilog stages**
- **simple CPU**
- **RISC-V single-cycle**
- **pipeline hazards/forwarding**

That ordering matches both your background and the repo’s teaching structure.

If you want, I can do a **second pass through this repo and turn this into a week-by-week study schedule** with exact files to read each day.
