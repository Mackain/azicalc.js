# AziCalc.js - Sunrise & Sunset Azimuth Calculator

This JavaScript module provides functions to calculate the **compass azimuth (degrees from true north)** of both **sunrise** and **sunset** on four key celestial events:

- **Spring Equinox**
- **Summer Solstice**
- **Fall Equinox**
- **Winter Solstice**

The functions take as input:
- **Latitude** (degrees)
- **Longitude** (degrees)
- **Year** (e.g., 2025, 1, -10000)

It accounts for the **variation of Earth’s axial tilt (obliquity of the ecliptic)** over long time scales, so it can compute sunrise and sunset directions accurately even thousands of years in the past or future.

---

## 📐 The Mathematics Behind It

### 1. Solar Declination
The Sun’s declination (δ) is the angle between the rays of the Sun and the plane of the Earth’s equator.

- On the **equinoxes** (spring and fall), δ = 0°.
- On the **solstices**, δ = ±ε, where ε is the Earth’s **axial tilt** (obliquity of the ecliptic).

The axial tilt is not fixed; it varies over a 41,000-year cycle between about 22.1° and 24.5°. For a given year, we approximate it using Jean Meeus’ *Astronomical Algorithms*:

```
ε = 84381.406'' - 46.836769T - 0.0001831T² + 0.00200340T³ - 0.000000576T⁴ - 0.0000000434T⁵
```

where:
- T = (year - 2000) / 100, centuries since J2000 epoch,
- ε is returned in arcseconds, then converted to degrees.

---

### 2. Sunrise Azimuth Formula
The azimuth of **sunrise** (A_rise) (measured clockwise from **true north**) is given by:

```
cos(A_rise) = (sin(δ) - sin(φ) * sin(h)) / (cos(φ) * cos(h))
```

where:
- φ = latitude (radians)
- δ = solar declination (radians)
- h = -0.833° = apparent altitude of the Sun’s center at sunrise (accounts for refraction and solar radius)

Finally:

```
A_rise = arccos(cos(A_rise))
```

The result is converted to degrees.

- **90°** → due East
- **< 90°** → north of east (summer sunrise in Northern Hemisphere)
- **> 90°** → south of east (winter sunrise in Northern Hemisphere)

---

### 3. Sunset Azimuth Formula
The azimuth of **sunset** (A_set) is symmetric across the **meridian (South)** relative to sunrise.

That is:

```
A_set = 180° - (A_rise - 90°)
       = 270° - (90° - A_rise)
```

This ensures:
- If sunrise is **north of east** (< 90°), sunset will be **north of west** (> 270°).
- If sunrise is **south of east** (> 90°), sunset will be **south of west** (< 270°).

Thus sunrise and sunset positions are mirror images about the north-south axis.

---

## 🚀 Example Usage
```javascript
console.log("Spring Equinox Sunrise 2025:", springEquinoxSunrise(40.7, -74, 2025));
console.log("Spring Equinox Sunset 2025:", springEquinoxSunset(40.7, -74, 2025));

console.log("Summer Solstice Sunrise 2025:", summerSolsticeSunrise(40.7, -74, 2025));
console.log("Summer Solstice Sunset 2025:", summerSolsticeSunset(40.7, -74, 2025));

console.log("Fall Equinox Sunrise 2025:", fallEquinoxSunrise(40.7, -74, 2025));
console.log("Fall Equinox Sunset 2025:", fallEquinoxSunset(40.7, -74, 2025));

console.log("Winter Solstice Sunrise 2025:", winterSolsticeSunrise(40.7, -74, 2025));
console.log("Winter Solstice Sunset 2025:", winterSolsticeSunset(40.7, -74, 2025));

console.log("Summer Solstice Sunrise Year 1:", summerSolsticeSunrise(40.7, -74, 1));
console.log("Summer Solstice Sunset Year 1:", summerSolsticeSunset(40.7, -74, 1));

console.log("Summer Solstice Sunrise Year -10000:", summerSolsticeSunrise(40.7, -74, -10000));
console.log("Summer Solstice Sunset Year -10000:", summerSolsticeSunset(40.7, -74, -10000));
```

---

## 🌍 Applications
- Archaeoastronomy (studying ancient alignments with solar events)
- Solar energy planning
- Navigation and cartography
- Educational astronomy tools

---

## ⚠️ Limitations
- This model assumes ideal atmospheric refraction (-0.833°) but does not account for local weather, terrain, or horizon altitude.
- Longitude is currently accepted but not used in the formula, since azimuth at sunrise/sunset depends primarily on latitude and solar declination. For exact event **times**, longitude would be needed.
- Precession of equinoxes is not explicitly modeled here, but the axial tilt variation is included, which dominates the long-term shift of solstice sunrise and sunset azimuths.

---

## 📚 References
- Jean Meeus, *Astronomical Algorithms* (2nd Edition)
- NOAA Solar Position Calculator
- Astronomical Almanac

---

With this library, you can explore how sunrise and sunset directions change not only across the seasons, but also across millennia.
