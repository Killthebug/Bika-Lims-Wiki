From the [manual](http://demo.bikalabs.com/knowledge-centre/manual/bika-3-user-manual/setup-and-configuration/analysis-services):

The intercept table of Uncertainties is used to determine the measure of uncertainty for the analysis. These uncertainty values are shown throughout the system as +/- value, after the result

# Calculate precision from uncertainties

The system also allows to compute the precision and round the results according to the uncertainties. The rounding applies both for decimal results and results with scientific notation.

The decimal position is given by the first number different from zero in the uncertainty, at that position the system will round up the uncertainty and results. For example,
* For a result 5.243 and an uncertainty of 0.22, the system will display correctly as 5.2 +- 0.2. 
* For a value like 13.5 and an uncertainty of 1.34, the system will display correctly as 13 +- 1. 
* For a value like 0.0077 with an uncertainty 0.0021, the system will display 0.008 +- 0.002. 

If the "Calculate precision from uncertainties" is enabled in the Analysis service, and
* the non-decimal number of digits of the result is above the service's "Exponential Format Precision" value, the result will be formatted in scientific notation.

  Example: Given an Analysis with an uncertainty of 37 for a range of results between 30000 and 40000, with an Exponential Format Precision value equal to 4 and a result of 32092, the system will display the value as 3.2092E+04.

  _Refer to Bika Setup's "Results Reports" and "Analyses" sections for further information about the available scientific notation formats._
* or the number of digits of the integer part of the result is below the "Exponential Format Precision" value, the result will be formatted as decimal notation and the result will be rounded in accordance to the calculated precision from the uncertainty.

  Example: Given an Analysis with an uncertainty of 0.22 for a range of results between 1 and 10 with an Exponential Format Precision value equal to 4 and a result of 5.234, the system will display the value as 5.2. 
 
If the "Calculate precision from Uncertainties" is disabled in the analysis service, the same rules described above applies, but the precision used for rounding the result is not calculated from the uncertainty. The fixed length precision is used instead.

## How does the introduction of results _really_ work?

The system works in two different planes, the first one works while the user is introducing the results, the second one is when submitting the results.

Let's suppose Alkalinity has been defined as:

* _Precision as number of decimals_ = 3
* _Uncertainty_ = min:0/ max: 10/ Uncertainty value:0.33
* _Calculate precision from uncertainties_ is **enabled**
* _Allow manual uncertainty value input_ is **enabled**

Let's suppose Calcium has been defined as:

* _Precision as number of decimals_ = 2
* _Uncertainty_ not defined
* _Calculate precision from uncertainties_ is disabled
* _Allow manual uncertainty value input_ is disabled

During the Alkalinity results' introduction process the system makes some changes by itself:

1. If the user introduces a result between the min/max Uncertainty, the system will display the uncertainty value automatically in the uncertainty box. The result won't change its value during the results' introduction even if the _Calculate precision from uncertainties_ check-box has been enabled.
2. If the user introduces a value bigger than the min/max uncertainty, the uncertainty value won't be displayed.
3. Since the _allow manual uncertainty_ value input is enabled for this service, we can introduce a different uncertainty.

During the Calcium results' introduction process, nothing happens.

After the results' introduction process we can submit for verification the results and the system works like that:

* Alkalinity:
  If the results was contained between the uncertainty values, then the result will display the result and the uncertainty rounded.
  If the result was bigger than the uncertainty range, than the result will be rounded by the precision as number of decimals.

* Calcium:
  The result will be rounded using the precision as number of decimals.