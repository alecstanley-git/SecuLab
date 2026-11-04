Based on SecuLab - eugeneg88/SecuLab

# SecuLab - Solves secular equations of motion with various additional effects

Welcome to SecuLab!

SecuLab is a Python package for the integration of the secular equations of motion of three-body hierarchical systems.
Written by Alec Stanley and based on Evgeni Grishin's original code for studying various dynamical configurations focusing on hierarchical triples.

The options for secular equations include
---

* **Secular evolution to octupole order**

* **Additional fourth body**

* **Single-averaging corrections**

Based on `Luo et al. (2016) <http://adsabs.harvard.edu/abs/2016MNRAS.458.3060L>`_and `Grishin et al. (2018) <http://adsabs.harvard.edu/abs/2018MNRAS.481.4907G>`_.

The options for additional forces include
---

**Conservative short-range forces**
(GR, tides), based on `Liu et al. (2015) <http://adsabs.harvard.edu/abs/2015MNRAS.447..747L>`_.

**Tidal friction**
Equilibrium tides from Hut (1981) (e.g. Fabrycky and Tremane (2007) for more modern equations). In addition, a simplified version of dynamical tides is possible (e.g. Moe and Kratter, 2018), with some modifications.

**Gravitational wave emission and inspiral**
Compared to Peters (1964) formulae

**Simplified Galactic tides**
For wide binaries, based on Heisler and Tremaine (1986)

Clone / Download
---

You need to have git installed. In addition, you need the NumPy and SciPy Python packages, and Matplotlib for plotting.
Some tests require comparison with direct N-body code. I'm using  `REBOUND <https://rebound.readthedocs.io/en/latest/>`_, a high order accurate integrator.

This command will create a folder of SecuLab. That's pretty much it!

```
git clone https://github.com/alecstanley-git/seculab.git
```

Tests
---

This is still under construction!
To test some of the features, I've created some scripts with test cases.

Quadrupole evolution
---

This test should reproduce Figure 4. of  `Naoz et al. (2013) <http://adsabs.harvard.edu/abs/2013MNRAS.431.2155N>`_(Also Fig 3. in the `recent review <https://www.annualreviews.org/doi/10.1146/annurev-astro-081915-023315>`_ ).

```
   import sl_tests as slt
   rebound_flag = True; t_end_myr = 1
   slt.test_quadupole_tpq(rebound_flag,t_end_myr)
```

After about 5 minutes of integration you should get something like this:

![Test Quadrupole](test_quadupole_tpq.png)

You can turn off the N_body comparison by setting

```
   rebound_flag = False
```

Which will speed up the integration. You can also control the end time of the integration by changing `t_end_myr`.

Circumbinary planets
---

This script reproduces Fig. 3 of `Martin and Triaud (2016) <http://adsabs.harvard.edu/abs/2016MNRAS.455L..46M>`_:

```
   import sl_tests as slt
   rebound_flag = True; t_end_myr = 1; single_averaging_flag = False;
   incs = [157, 158, 159]
   slt.test_circumbinary_planets(rebound_flag,t_end_myr, single_averaging_flag, incs)
```

It might take about an hour to integrate with REBOUND the ~ 10^7 orbits up to 1 Myr, but eventually you will see something like this

![Circumbinary Planets SA OFF](circumbinary_planets_sa_off.png)

The dashed lines are the exact N-body while the solid lines are SecuLab integration. The results will fit a little better if we turn on the effective single averaging correction (more on that later!)

```
 single_averaging_flag = True;
```

This will reproduce a similar plot, only the first spikes are captured slightly better.

![Circumbinary Planets SA ON](circumbinary_planets_sa_on.png)

Effective single averaging
---

It is possible to add an effective force / potential that mimics the secular evolution with corrections from short-term variations of the orbital elements. The corrections is based on `Luo et al. (2016) <http://adsabs.harvard.edu/abs/2016MNRAS.458.3060L>`_and `Grishin et al. (2018) <http://adsabs.harvard.edu/abs/2018MNRAS.481.4907G>`_.

The following test reproduces Fig. 5 of `Luo et al. (2016) <http://adsabs.harvard.edu/abs/2016MNRAS.458.3060L>`_:

```
  import sl_tests as slt
  rebound_flag = True
  t_end_myr = 0.5  
  slt.test_single_averaging(rebound_flag, t_end_myr)
```

This should reproduce the following plot:

![Single Averaging](single_averaging.png)

Turning on single-averaging corrections could really matter!
