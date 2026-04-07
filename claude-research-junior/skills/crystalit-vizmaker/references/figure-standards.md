# Figure Standards Reference

Publication-quality figure specifications for Nature, Lancet Digital Health, and npj Digital Medicine.

## Dimensional Standards

| Property | Value | Notes |
|----------|-------|-------|
| Single-column width | 8-9 cm | Most subpanel figures |
| Double-column width | 10-12 cm | Wide figures (heatmaps, networks) |
| Maximum height | 20 cm | Full-page figures only |
| Resolution (PNG) | 300 DPI | Minimum for print |
| Resolution (PDF) | Vector | Always preferred for line art |
| Padding | 0.02 inches | Minimal whitespace via `pad_inches` |

## Typography

| Element | Size | Notes |
|---------|------|-------|
| Axis labels | 9-10 pt | Clear, readable at print size |
| Tick labels | 8-9 pt | Minimum readable |
| Legend text | 8-9 pt | Match tick labels |
| Annotations | 8 pt | Absolute minimum |
| Title (if used) | 10-11 pt | Usually handled in figure caption instead |

Font family: DejaVu Sans, Arial, or Helvetica (sans-serif only).

## Color Palettes

### Primary palette (colorblind-friendly)

```python
PALETTE = ['#2196F3', '#FF9800', '#4CAF50', '#F44336', '#9C27B0',
           '#00BCD4', '#FFC107', '#795548', '#607D8B', '#E91E63']
```

### For sequential data: `viridis`, `cividis`, or `plasma`
### For diverging data: `coolwarm` or `RdBu`
### For categorical data: `tab10` or the custom palette above

## Abbreviation Map Template

Build a domain-specific abbreviation map. Here is a template from cardiac CT:

```python
ABBREV_MAP = {
    'CT_Pulmonary_Angiography_CTPA': 'CTPA',
    'Left_Ventricle': 'LV',
    'Right_Ventricle': 'RV',
    'Left_Atrium': 'LA',
    'Right_Atrium': 'RA',
    'Statistical_Shape_Model_SSM': 'SSM',
    'Dice_Similarity_Coefficient': 'Dice',
    'Hausdorff_Distance': 'HD',
    'Mean_Surface_Distance': 'MSD',
    # ... extend for your domain
}
```

## Layout Guidelines

### Bar charts
- Horizontal orientation preferred (labels read naturally)
- Sort by value (descending) unless chronological ordering makes more sense
- Include value labels on bars if space permits

### Heatmaps
- Use diverging colormap for correlation matrices
- Use sequential colormap for co-occurrence counts
- Always include a colorbar with label
- Rotate x-axis labels 45° if needed; prefer abbreviations over rotation

### Network graphs
- Circular layout for dense networks (avoids central clustering)
- Place labels outside the node circle
- Use edge width to encode co-occurrence strength
- Group related nodes by angular position

### Treemaps
- Label only cells above 3% of total area
- Use "Theme Name\nN subthemes\nM concepts" format for theme-level treemaps
- Ensure text contrast against cell background color

### Radar charts
- Maximum 8-10 axes (more becomes unreadable)
- Fill the area with low alpha (0.25)
- Label axes at the outer edge, not inside the chart

## Anti-Patterns to Avoid

1. **3D charts** — Never. They distort perception of values.
2. **Pie charts** — Almost never. Bar charts convey the same information more accurately.
3. **Excessive gridlines** — Remove or make very light (alpha 0.15).
4. **Default matplotlib style** — Always apply custom styling.
5. **Legend outside figure** — Increases figure size unpredictably. Place inside, in the least-busy quadrant.
6. **Dual y-axes** — Misleading. Use side-by-side subplots instead.
