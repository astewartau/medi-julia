# MEDI Julia Implementation

A Julia implementation of the Morphology Enabled Dipole Inversion (MEDI) method for quantitative susceptibility mapping (QSM).

## Prerequisites

- Julia (tested with Julia 1.x)
- Required packages will be automatically installed via the Project.toml

## Installation

1. Clone this repository
2. Navigate to the project directory
3. Install dependencies:
   ```bash
   julia --project=. -e "using Pkg; Pkg.instantiate()"
   ```

## Usage

### Quick Start

Run the main script:
```bash
./run_medi.jl
```

Or alternatively:
```bash
julia run_medi.jl
```

### Using the MEDI Function

The main function is `MEDI()` defined in `medi.jl`. Basic usage:

```julia
using Pkg
Pkg.activate(@__DIR__)
include("medi.jl")

# Call MEDI with your data
chi = MEDI(RDF, N_std, iMag, Mask, voxel_size; 
          lambda=1000.0, B0_dir=3, merit=false, smv=false)
```

### Parameters

**Required arguments:**
- `RDF`: 3D field map in radians
- `N_std`: Noise standard deviation map (same size as RDF)
- `iMag`: Anatomical magnitude image
- `Mask`: Binary 3D mask (Bool array) of region of interest
- `voxel_size`: Tuple of voxel dimensions in mm

**Keyword arguments:**
- `lambda`: Regularization parameter (default: 1000.0)
- `B0_dir`: Magnetic field direction - integer 1,2,3 or 3-element vector (default: 3)
- `merit`: Enable iterative merit-based adjustment (default: false)
- `smv`: Apply spherical mean value preprocessing (default: false)
- `radius`: SMV radius in mm (default: 5.0)
- `data_weighting`: Data weighting mode 0 or 1 (default: 1)

## Project Structure

- `medi.jl` - Main MEDI implementation
- `run_medi.jl` - Executable script
- `Project.toml` - Julia package dependencies
- Individual component files:
  - `fgrad.jl` - Forward gradient calculation
  - `bdiv.jl` - Backward divergence
  - `sphere_kernel.jl` - Spherical mean value operations
  - `dipole_kernel.jl` - Dipole kernel functions
  - `dataterm_mask.jl` - Data term masking
  - `gradient_mask.jl` - Gradient masking
  - `cgsolve.jl` - Conjugate gradient solver