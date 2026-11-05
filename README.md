# FNO thermoelastic response dataset

2400 simulated samples used for FNO training, validation and testing in the
associated study of porous functionally graded plates.

## Files

- `mgt_2400_samples.mat`: material profiles, complex responses, grid and controls
  (MATLAB v7.3/HDF5).
- `mgt_2400_metadata.csv`: sample parameters and fixed data partitions.

## Data layout

Each sample contains 385 points over the normalized thickness coordinate
`xOverH` = X/h, from 0 to 1. Array dimensions are:

| Variable | MATLAB | h5py |
|---|---|---|
| `materialProfiles` | 2400 x 385 x 8 | 8 x 385 x 2400 |
| `responseRe`, `responseIm` | 2400 x 385 x 6 | 6 x 385 x 2400 |
| `controls` | 2400 x 8 | 8 x 2400 |
| `splitCode` | 2400 x 1 | 1 x 2400 |
| `xOverH` | 385 x 1 | 1 x 385 |

**Material-channel order:** E (Pa), rho (kg/m^3), nu, alpha (1/K), kappa
(W/(m K)), c_e (J/(kg K)), actual porosity phi, ceramic fraction Vc.
The quantities nu, phi and Vc are dimensionless.

**Response order:** Theta, U=u_Z, V=v_X, sigma_XX, sigma_ZZ, sigma_XZ.
`responseRe` and `responseIm` store their real and imaginary parts.
The corresponding physical units are K, m, m, Pa, Pa and Pa.

**Heat-flux scaling:** saved responses are divided by
q0 = 10,000 W/m^2. Recover the physical complex fields at this loading as

    field = 10000 * (responseRe + i * responseIm).

The saved responses have not been statistically standardized.

**Control order:** n, e0, eta, ellOverH, zeta, tauMOmega, rM, q0_W_m2.
Here eta is the ratio in s = -eta*Omega - i*Omega, and ellOverH = ell/h.
Fixed thermal parameters are tau0*Omega = 0.70 and
rM = tau0*kappa_star/kappa = 0.07. Further field names and fixed simulation
settings are stored in `datasetInfo`; model definitions follow the manuscript.

## Fixed partitions

`splitCode`: 1 = training (1920), 2 = validation (240), 3 = test (240).
The CSV column `split` records the same assignments. CSV `sampleId` starts
at 1 and follows the sample order in the MAT arrays; use `sampleId - 1`
for zero-based HDF5 sample indexing.

