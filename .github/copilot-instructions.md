# ModelCheck World of Blocks - AI Coding Guide

## Project Overview

This is a formal verification project using NuSMV to solve an advanced version of the classic "blocks world" problem with variable dimensions. The methodology is "Planning as Model Checking" - instead of traditional planning languages, we use NuSMV to model the world and verify if goal states are reachable through counterexample generation.

### 🎯 **Project Status: SITUATION 1 COMPLETED** ✅

**All 4 objectives (SF1, SF2, SF3, SF4) successfully solved and verified:**

- SF1: 7 steps (Complex tower configuration)
- SF2: 7 steps (Stepped configuration)
- SF3: 3 steps (Most efficient - distributed layout)
- SF4: 5 steps (Linear configuration)

### Key Innovation: Physical Dimensions & Coordinate System

Unlike the classic abstract blocks problem, this version introduces:

- **Variable block sizes**: `size_a = 1`, `size_b = 1`, `size_c = 2`, `size_d = 3`
- **Coordinate-based world**: (x,y) grid system where exact positions matter
- **6-position field**: coordinates 0, 1, 2, 3, 4, 5 (NOT 0-6!)
- **No stability constraints**: Any block can be placed on any other (based on visual diagrams authority)

## Planning as Model Checking Methodology

### Core Strategy: Inverted Logic

1. **Model the Universe**: Define all possible states, actions, and transitions in NuSMV
2. **Ask the Inverted Question**: Use `CTLSPEC !EF(goal)` to ask "Is it IMPOSSIBLE to reach the goal?"
3. **Interpret Results**:
   - `is false` → Goal is reachable, NuSMV provides counterexample (the plan)
   - `is true` → Goal is genuinely unreachable under defined rules

### Model Structure Pattern

All `.smv` files follow this modular structure:

- **DEFINE section**: Block sizes as constants (`size_a := 1`, `size_b := 1`, `size_c := 2`, `size_d := 3`)
- **VAR section**: Block positions using coordinate system (`pos_[block]_x`, `pos_[block]_y`)
- **IVAR section**: Input actions (`action: {none, move_a, move_b, move_c, move_d}`)
- **Predicates**: Spatial relationship macros (`is_[block1]_on_[block2]`, `is_clear_[block]`)
- **ASSIGN section**: Initial state and transition logic with collision detection
- **CTLSPEC**: Goal verification using `!EF(goal)` pattern

### Two Modeling Approaches

1. **Coordinate-based** (`alexandre/` directory): Uses x,y coordinates with complex collision detection
2. **Symbolic** (`andre/model.smv`): Uses discrete positions (`table0..table6`, `on_a`, `on_b`, etc.)

## Critical Development Patterns

### Physical Rules (Authoritative from Visual Diagrams)

- **Block sizes**: `size_a = 1`, `size_b = 1`, `size_c = 2`, `size_d = 3` (fixed, based on diagrams)
- **NO stability constraints**: Any block can be placed on any other block (diagrams override text specs)
- **Clear block rule**: Only blocks with nothing above them can be moved
- **Collision detection**: Blocks cannot overlap in the same (x,y) coordinate space

### Coordinate System Implementation

- Grid bounds: x-axis (0..6), y-axis (0..3/4) depending on situation
- Collision logic: `!(pos_x >= other_x + size | pos_x + size <= other_x)`
- Support validation: Blocks need underlying support unless on ground (y=0)

### Transition Logic Template

```smv
next(pos_x) := case
    action = move_block & is_clear_block &
    [collision_checks] &
    [support_validation] : target_x;
    TRUE: pos_x;
esac;
```

### State Validation Patterns

- Initial states must be physically valid (no floating blocks)
- Goal states define target configurations to prove reachable/unreachable
- Comments indicate "ajustado" (adjusted) when initial states needed correction

## Development Workflow

### File Naming Convention

- `situacao[N].smv`: Different problem scenarios with varying initial/goal states
- Comments indicate "VERSÃO FINAL CORRIGIDA" for debugged versions
- Coordinate models use grid bounds (0..6 x-axis, 0..3/4 y-axis)

### Model Checking Commands

Use NuSMV model checker:

```bash
nusmv [filename].smv
```

### Three Situations to Solve

Based on the attached specifications:

- **Situação 1**: Multiple goal states (SF1, SF2, SF3, SF4) from initial S0
- **Situação 2**: Goal S7 from initial S0
- **Situação 3**: Goal S7 from initial S0

### Key Verification Patterns

- `CTLSPEC !EF(goal)` proves goal state reachability from initial state
- `is false` = goal reachable (counterexample provides the plan)
- `is true` = goal unreachable under current model constraints
- Focus on precondition validation in complex transition logic

## Integration & Dependencies

- Pure NuSMV models (no external dependencies)
- Models are self-contained formal specifications
- `Rascunhe/` contains planning materials and documentation

## Debugging Guidelines

- Check size constants match problem specification (`size_d = 3` always)
- Verify initial state stability (no floating blocks)
- Validate collision detection logic in complex transition cases
- Ensure goal state coordinates are physically reachable
- Remember: Visual diagrams are authoritative over text specifications
- Test with different coordinate bounds if model seems too restrictive
