# datefixR: Standardize Dates in Different Formats or with Missing Data

There are many different formats dates are commonly represented with:
the order of day, month, or year can differ, different separators ("-",
"/", or whitespace) can be used, months can be numerical, names, or
abbreviations and year given as two digits or four. `datefixR` takes
dates in all these different formats and converts them to R's built-in
date class. If `datefixR` cannot standardize a date, such as because it
is too malformed, then the user is told which date cannot be
standardized and the corresponding ID for the row. `datefixR` also
allows the imputation of missing days and months with user-controlled
behavior.

Get started by reading
[`vignette("datefixR")`](https://docs.ropensci.org/datefixR/articles/datefixR.md)

## See also

Useful links:

- <https://docs.ropensci.org/datefixR/>

- <https://github.com/ropensci/datefixR/>

- Report bugs at <https://github.com/ropensci/datefixR/issues>

## Author

**Maintainer**: Nathan Constantine-Cooke
<nathan.constantine-cooke@ed.ac.uk>
([ORCID](https://orcid.org/0000-0002-4437-8713))

Other contributors:

- Jonathan Kitt <jonathan.kitt@protonmail.com> \[contributor,
  translator\]

- Antonio J. Pérez-Luque <ajpelu@gmail.com>
  ([ORCID](https://orcid.org/0000-0002-1747-0469)) \[contributor,
  translator\]

- Daniel Possenriede <possenriede+r@gmail.com>
  ([ORCID](https://orcid.org/0000-0002-6738-9845)) \[contributor,
  translator\]

- Michal Lauer <michal.lauer.25@gmail.com> \[contributor, translator\]

- Kaique dos S. Alves <kaiquedsalves@gmail.com>
  ([ORCID](https://orcid.org/0000-0001-9187-0252)) \[reviewer\]

- Al-Ahmadgaid B. Asaad <alahmadgaid@gmail.com>
  ([ORCID](https://orcid.org/0000-0003-3784-8593)) \[reviewer\]

- Anatoly Tsyplenkov <atsyplenkov@gmail.com>
  ([ORCID](https://orcid.org/0000-0003-4144-8402)) \[contributor,
  translator\]

- Chitra M. Saraswati <chitra.m.saraswati@gmail.com>
  ([ORCID](https://orcid.org/0000-0002-8159-0414)) \[contributor,
  translator\]
