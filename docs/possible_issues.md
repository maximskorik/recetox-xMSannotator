# Potential Issues
This list contains potential bugs & problems detected in the original code while refactoring.

* [check_element](https://github.com/RECETOX/recetox-xMSannotator/blob/master/R/check_element.R) detects `C` if formula contains `Cl` -> No compounds are filtered because they don't contain carbon
* [multilevelannotationstep5](https://github.com/RECETOX/recetox-xMSannotator/blob/c7ad3bb2f4e7cc6aa35d8a0804fe6ef7a8389729/R/multilevelannotationstep5.R#L77-L101): 
   * the `.combine = rbind` parameter of `foreach::foreach()` function causes a twisted output, collapsing a `174x60` 
  matrix to a `61x10` matrix -> 61 are the non-empty rows, but the entries in rows that have more than 10 columns filled get discarded;
   * the issue described above may cause `multilevelannotationstep5` to compute different outputs depending on whether 
  the loop is executed _in parallel_ (`%dopar%`) or _sequentially_ (`%do%`). The results may differ in **chemical scores** or the **number 
  of annotations**. When chemical scores differ, _sequential_ execution will systematically output lower scores.
* in [multilevelannotationstep4](https://github.com/RECETOX/recetox-xMSannotator/blob/master/R/multilevelannotationstep4.R) (and functions called by it) are some suspicious conditions where they check for `curdata$score < 10` and `curdata$score > 10`, but never for actual value `10`
