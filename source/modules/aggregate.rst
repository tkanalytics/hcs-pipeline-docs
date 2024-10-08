Aggregation
===========

Overview:
^^^^^^^^^

The **Aggregation Module** allows users to aggregate data by grouping on
selected metadata columns. The module calculates various summary
statistics such as the mean, standard deviation (STD), standard error
(SE), coefficient of variation (CV), and the count of observations.
Additionally, users have the option to calculate statistical measures
such as p-values, SSMD (Strictly Standardized Mean Difference), and
Z-prime, comparing the data to a specified negative control.

Purpose:
^^^^^^^^

This module is designed to help users group their data by selected
metadata columns and compute important summary statistics for further
analysis. For biologists, this is especially useful when working with
experimental data from high-throughput screens or large datasets where
data needs to be summarized or compared across different conditions or
treatments.

--------------

Inputs and Options:
~~~~~~~~~~~~~~~~~~~

1.  **DataFrame Selection**:

    -  **Functionality**: The user selects a dataset from the available
       data sources. This dataset will be the base for the aggregation.
    -  **Validation**: The dataset must contain relevant metadata and
       numerical columns that can be aggregated.

2.  **Checkbox: Metadata column inference**:

    -  **Default**: Unchecked.
    -  **Functionality**: When checked, it automatically selects columns
       that start with the prefix ``Metadata_`` as potential metadata
       columns. These columns are typically used to store non-numeric
       data related to experimental setup or grouping (e.g., treatment
       conditions, timepoints).
    -  **Use Case**: Simplifies the process of identifying metadata
       columns when the dataset follows a naming convention.

3.  **Checkbox: Remove CellProfiler features**:

    -  **Default**: Unchecked.
    -  **Functionality**: If enabled, the module filters out columns
       commonly associated with CellProfiler features (e.g.,
       ``AreaShape``, ``Texture``, ``Granularity``), allowing the user
       to focus only on metadata columns during the aggregation process.
    -  **Use Case**: Useful when working with data generated by
       CellProfiler, where a large number of features can make it
       difficult to find metadata columns.

4.  **Multiselect: Select metadata columns**:

    -  **Functionality**: The user selects which metadata columns will
       be used to group the data during aggregation. These columns help
       define how the data is summarized (e.g., grouping by timepoint,
       treatment, plate).
    -  **Use Case**: Allows users to customize how data is aggregated,
       depending on experimental factors or groupings of interest.

5.  **Multiselect: Select grouping columns**:

    -  **Functionality**: The user selects columns to define the groups
       for aggregation. These columns are often the same as metadata
       columns but can be adjusted for specific groupings.
    -  **Use Case**: Groups data based on selected conditions (e.g., all
       wells from a specific plate and timepoint) for summary statistic
       calculations.

6.  **Checkbox: Calculate p-values and SSMD**:

    -  **Default**: Unchecked.
    -  **Functionality**: If checked, the module calculates additional
       statistical measures like p-values and SSMD (Strictly
       Standardized Mean Difference) to compare experimental groups
       against a control.
    -  **Use Case**: Useful for assessing the statistical significance
       of observed differences in data, especially when comparing
       treatments or controls.

7.  **Selectbox: Select the negative control**:

    -  **Functionality**: If p-values and SSMD are enabled, the user can
       choose a specific condition or group (from the selected grouping
       columns) to serve as the negative control for the statistical
       tests.
    -  **Use Case**: Defines the reference group against which other
       groups are compared.

8.  **Button: Add negative control**:

    -  **Functionality**: Allows the user to add multiple negative
       controls for comparison across different conditions.
    -  **Use Case**: Useful when users want to perform multiple
       statistical comparisons, each with different control groups.

9.  **Dataset Name Input**:

    -  **Functionality**: The user can provide a name for the dataset.
       This name is used when saving the aggregated data to make it
       accessible for future analysis steps.
    -  **Use Case**: Helps organize and label datasets for later use.

10. **Button: Run Aggregation**:

    -  **Functionality**: This button initiates the aggregation process
       and calculates the requested statistics based on the user’s
       input.

--------------

Detailed Explanation of Computation:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Aggregation Functions**:

   -  The dataset is grouped by the user-specified metadata columns, and
      several aggregation functions are applied to the non-metadata
      (numerical) columns:

      -  **Mean**: Computes the average value of the data in each group.
      -  **Standard Deviation (STD)**: Measures the dispersion or spread
         of values around the mean.
      -  **Standard Error (SE)**: The standard error of the mean, which
         accounts for sample size.
      -  **Coefficient of Variation (CV)**: The ratio of the standard
         deviation to the mean, indicating relative variability.
      -  **Count**: The number of data points within each group.


--------------

Mathematical Formulas:
~~~~~~~~~~~~~~~~~~~~~~

1. **Mean (μ)**: The mean is the sum of all values in a group divided by
   the number of values: [ :math:`\mu = \frac{1}{n}`
   :math:`\sum\_{i=1}^{n} x_i `]

   -  Where (x_i) is the individual data point and (n) is the total
      number of data points in the group.

2. **Standard Deviation (σ)**: The standard deviation measures the
   spread of data points around the mean: [ :math:`\sigma = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (x_i - \mu)^2}` ]

   -  Where ( :math:`\mu `) is the mean of the group, and (n) is
      the sample size.

3. **Standard Error (SE)**: The standard error of the mean is the
   standard deviation divided by the square root of the sample size: [
   SE = :math:`\frac{\sigma}{\sqrt{n}}` ]

   -  Where ( :math:`\sigma `) is the standard deviation, and (n)
      is the sample size.

4. **Coefficient of Variation (CV)**: The CV is the ratio of the
   standard deviation to the mean: [ :math:`CV = \frac{\sigma}{\mu}` ]

   -  This is useful for comparing the relative variability between
      groups, as it normalizes the standard deviation with respect to
      the mean.

5. **Count**: The number of observations in each group is calculated
   using the ``count`` function.

--------------

Statistical Measures: P-values, SSMD, and Z-prime
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the aggregation functions, the module provides options
for calculating p-values, SSMD (Strictly Standardized Mean Difference),
and Z-prime, which are often used to compare experimental data against a
control group.

1. **P-values**:
^^^^^^^^^^^^^^^^

The module uses a two-sample t-test to compute p-values. This compares
the means of two groups to determine if they are statistically
different.

The p-value is computed as: [ t =
:math:`\frac{\mu_1 - \mu_2}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}}`
] Where: - (:math:`\mu`\_1, :math:`\mu`\_2) are the means of
the two groups. - (:math:`\sigma`\_1, :math:`\sigma`\_2) are
the standard deviations of the two groups. - (n_1, n_2) are the sample
sizes of the two groups.


2. **Strictly Standardized Mean Difference (SSMD)**:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

SSMD is a measure of effect size used in high-throughput screens. It is
defined as: [ SSMD =
:math:`\frac{\mu_1 - \mu_2}{\sqrt{\sigma_1^2 + \sigma_2^2}}` ]
Where: - (:math:`\mu`\_1, :math:`\mu`\_2) are the means of the
two groups. - (:math:`\sigma`\_1, :math:`\sigma`\_2) are the
standard deviations of the two groups.


3. **Z-prime**:
^^^^^^^^^^^^^^^

Z-prime is a statistical measure used to evaluate the quality of a
screening assay. It measures the separation between positive and
negative controls: [ Z’ = 1 -
:math:`\frac{3(\sigma_1 + \sigma_2)}{|\mu_1 - \mu_2|}` ] Where: -
(:math:`\mu`\_1, :math:`\mu`\_2) are the means of the two
groups. - (:math:`\sigma`\_1, :math:`\sigma`\_2) are the
standard deviations of the two groups.

--------------

Example Usage Scenarios:
~~~~~~~~~~~~~~~~~~~~~~~~

1. **Scenario 1: Aggregating Timepoint Data Across Plates**: A biologist
   has data from multiple plates, each representing a different
   timepoint. Using this module, they can group the data by ``Plate``
   and ``Timepoint`` columns, calculating the mean, standard deviation,
   and other statistics for each combination of plate and timepoint.
   They can also add negative controls to assess the statistical
   significance of the timepoint effects.

2. **Scenario 2: Comparing Treatments with and without Additional
   Factors**: A biologist is studying the effects of different
   treatments, with some wells exposed to an additional factor (e.g., a
   drug). They can group the data by ``Treatment`` and ``Factor``
   columns and compare the groups using p-values and SSMD to assess the
   impact of the treatments on the observed results. Additionally,
   Z-prime can be calculated to evaluate the quality of the screening
   assay.

--------------

Conclusion:
~~~~~~~~~~~

This module provides powerful aggregation and statistical comparison
tools that are essential for summarizing and analyzing experimental
data. By offering a range of aggregation functions and statistical
measures, it enables biologists to quantify variability, assess
statistical significance, and evaluate the quality of their experimental
data.

Options and Flexibility:
~~~~~~~~~~~~~~~~~~~~~~~~

-  **Custom Grouping**: Users can select any combination of metadata
   columns for grouping, allowing flexible aggregation based on
   experimental design.
-  **Statistical Comparison**: With the ability to calculate p-values,
   SSMD, and Z-prime, users can quickly assess the significance of
   differences between experimental groups.
-  **Control Group Selection**: Provides flexibility to define different
   control groups for statistical comparison, allowing for multiple
   reference conditions to be tested in parallel.
