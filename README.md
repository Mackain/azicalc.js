# Sunrise & Sunset Azimuth Calculator

This JavaScript module provides functions to calculate the **compass azimuth (degrees from true north)** of both **sunrise** and **sunset** on four key celestial events:

- **Spring Equinox**  
- **Summer Solstice**  
- **Fall Equinox**  
- **Winter Solstice**

The functions take as input:  
- **Latitude** (degrees)  
- **Longitude** (degrees)  
- **Year** (e.g. 2025, 1, –10000)

It accounts for the **variation of Earth’s axial tilt (obliquity of the ecliptic)** over long time scales, so it can compute sunrise and sunset directions accurately even thousands of years in the past or future.

---

## 📐 The Mathematics Behind It

### 1. Solar Declination

The Sun’s declination, \( \delta \), is the angle between the rays of the Sun and the plane of the Earth’s equator.

- On the **equinoxes** (spring and fall), \( \delta = 0^\circ \).  
- On the **solstices**, \( \delta = \pm \epsilon \), where \( \epsilon \) is the Earth’s **axial tilt** (obliquity of the ecliptic).

The axial tilt is not fixed; it varies over a ~41,000-year cycle between about 22.1° and 24.5°. For a given year, we approximate it using Jean Meeus’ *Astronomical Algorithms*:

$$ \epsilon = 84381.406'' - 46.836769\,T - 0.0001831\,T^2 + 0.00200340\,T^3 - 0.000000576\,T^4 - 0.0000000434\,T^5 $$

where  

$$ T = \frac{\text{year} - 2000}{100} $$

Here, $\epsilon$ is computed in arcseconds and then converted to degrees.

---

### 2. Sunrise Azimuth Formula

The azimuth of **sunrise**, $\( A_{\text{rise}} \)$ (measured clockwise from **true north**), is given by:


$$ cos(A_{\text{rise}}) = \frac{\sin(\delta) - \sin(\phi)\,\sin(h)}{\cos(\phi)\,\cos(h)} $$


where  
- $\( \phi \)$ = latitude (in radians)  
- $\( \delta \)$ = solar declination (in radians)  
- $\( h = -0.833^\circ \)$ is the apparent altitude of the Sun’s center at sunrise (accounts for atmospheric refraction and the solar disc’s radius)

Then:


$$ A_{\text{rise}} = \arccos\bigl(\cos(A_{\text{rise}})\bigr)  $$

The result is converted to degrees. Interpreting the result:

- $\(90^\circ\)$ → due East  
- $\(< 90^\circ\)$ → north of East (e.g. summer sunrise in northern hemisphere)  
- $\(> 90^\circ\)$ → south of East (e.g. winter sunrise in northern hemisphere)

---

### 3. Sunset Azimuth Formula

The azimuth of **sunset**, $\( A_{\text{set}} \)$, is symmetric across the meridian relative to sunrise:


$$ A_{\text{set}} = 180^\circ - \bigl(A_{\text{rise}} - 90^\circ \bigr) = 270^\circ - \bigl(90^\circ - A_{\text{rise}} \bigr) $$

This symmetry ensures that sunrise and sunset positions mirror each other across the north–south axis:

- If sunrise is **north of east** $(\(< 90^\circ\))$, sunset will be **north of west** $(\(> 270^\circ\))$.  
- If sunrise is **south of east** $(\(> 90^\circ\))$, sunset will be **south of west** $(\(< 270^\circ\))$.

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

console.log("Summer Solstice Sunrise Year –10000:", summerSolsticeSunrise(40.7, -74, -10000));
console.log("Summer Solstice Sunset Year –10000:", summerSolsticeSunset(40.7, -74, -10000));
