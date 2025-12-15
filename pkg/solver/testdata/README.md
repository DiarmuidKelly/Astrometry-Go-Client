# Test Data for Integration Tests

This directory contains real astronomical images and ground truth data for integration testing.

## Files

### m42-orion.jpg
- **Subject**: Orion Nebula (M42) region
- **Camera**: Canon APS-C sensor
- **Lens**: 200mm EF lens (adapted to APS-C body, effective 320mm with 1.6x crop)
- **Size**: 6000x4000 pixels
- **Purpose**: Real astronomical image for validating plate-solving functionality

### wcs.fits
- **Source**: Astrometry.net solution for m42-orion.jpg
- **Format**: FITS file containing World Coordinate System (WCS) solution
- **Purpose**: Ground truth data for validating our solver produces matching results

### ground_truth.json
- **Purpose**: Structured ground truth data extracted from wcs.fits
- **Contents**:
  - RA/Dec coordinates (J2000)
  - Pixel scale (arcsec/pixel)
  - Field of view (degrees)
  - Rotation angle
  - Tolerance values for test validation

## Usage in Tests

Integration tests use this data to:
1. **Load** m42-orion.jpg as test input
2. **Solve** using all 3 Docker images (diarmuidk DockerHub, diarmuidk GHCR, dm90 legacy)
3. **Validate** results match ground truth within tolerance
4. **Verify** all 3 images produce consistent results

## Regenerating Ground Truth

If you need to regenerate ground truth data:

```bash
# 1. Solve image with astrometry.net (online or local)
#    This produces wcs.fits

# 2. Extract ground truth
astro-cli wcs-info wcs.fits --json > ground_truth_new.json

# 3. Manually format into ground_truth.json structure
```

## Notes

- **Tolerances** are set to account for minor differences between solver implementations
- **Position tolerance** (10 arcsec) allows for WCS calculation differences
- **Pixel scale tolerance** (5%) accounts for rounding and numerical precision
- All 3 Docker images should agree with each other within these tolerances
