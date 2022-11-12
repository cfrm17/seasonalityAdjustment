# Commodity Seasonality Adjustment

Natural gas risk factors (all-in and spreads) exhibit seasonal effects.  Ultimately, the scenarios generated are corrected for seasonality by effectively replacing the average volatility and correlation with the seasonal equivalents.  This is done by the following method:

Given average and seasonal covariance matrices, Cav and Cs, one can produce eigen-value decompositions as follows:

 

NOTE: A seasonal covariance matrix is one obtained by determining the covariance for returns from a given calendar month over 9 years.

Given these eigen-value decompositions, correlated deviates can be generated as:

 

Where,
 

NOTE: A seasonal volatility is one obtained by determining the volatility for returns from a given calendar month over 9 years.

Therefore, one can generate seasonal scenarios from average scenarios as follows:

 
 

where + indicates the pseudo-inverse.  

We call Ps the seasonal rotation matrix.  Cvol is called the seasonal volatility correction factor and is given by: 

 

Where   and   are the average implied volatility over 9 years for a given calendar month and for the average, respectively.
 

In computing the rotation matrices, the following process is used:
1.	make average and seasonal correlation matrices PSD
2.	compute eigen-values and eigen-vectors
3.	sort the eigen-values while maintaining the order of the eigen-vector matrix
4.	find the eigen-value cut-off that accounts for 99% of the variance (i.e. principle components analysis)
5.	convert all eigen-values that fall below the threshold to zero
6.	compute:

 

where,   is a diagonal matrix with the seasonal volatilities on the diagonal,   is a diagonal matrix with the square-root of the eigen-values of Cs,   is a matrix with columns equal to the eigen-vectors of Cs,   is a diagonal matrix with elements equal to the square-root of the eigen-values of Cav,   is a matrix with columns equal to the eigen-vectors of Cav, and   is a diagonal matrix with elements equal to the average volatilities.  Note that the second line of the equation is possible because of the orthogonality of Vav.

For all-in pipeline risk factors, both volatility and correlation are corrected for seasonality.  The average volatilities and correlations are based on 1 year of historical data, while the seasonal volatility and correlation are based on 9 years of historical data.  For pipeline spread risk factors, only the correlation is corrected for seasonality.  The rotation matrices are output as part of the calibration package.

The seasonal volatility and correlation corrections are separated.  That is, the rotation matrix is simply:

 .

The volatility is corrected with a correction factor by which the scenarios are multiplied.  The correction factor is:

 .

Natural gas pipeline at-the-money implied volatility risk factors receive an asymmetry (skew) adjustment.  That is, the scenarios generated from a symmetric double exponential undergo a transform to create asymmetric double exponential scenarios.  

The symmetric double exponential density function is given by:

 

  is given by the average absolute deviation of the returns:

 

The variance of a double exponential is given by:

 

The asymmetric double exponential density function is given by:

 

where   is the estimate of the exponential decay parameter for positive (negative) returns and   is the mode of the distribution (i.e. the point at which the positive and negative exponentials meet).  

Ultimately, the transformation of the scenarios is given by:

  if x<xc
  if x≥xc

where lower-case x is the original symmetric scenario, upper-case X is the new scenario, R is given by 
 

  is the symmetric volatility and is given by:

  
and xc is given by:

 

Therefore, the calibration must compute four parameters:
•	The symmetric volatility   which is already computed in section Error! Reference source not found.
•	The symmetric decay parameter  
•	The asymmetric positive decay parameter  
•	The asymmetric negative decay parameter  

This is calculated by assuming the following linear function for the parameter R:

  for T≤8 months
     for T>8 months where T is the contract maturity.

Studies showed that the estimation of the asymmetry parameters was only reliable for the first 7 contract maturities (months) and that the asymmetry decreased in linear fashion according to the equation above (see https://finpricing.com/lib/FiBondCoupon.html ).

The three decay parameters are then calculated as follows for the first 7 maturities of each PL implied volatility risk factor:

