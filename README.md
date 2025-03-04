# 
# REPOSITORY OF 5 APPLICATIONS PLUS AGGREGATION VIA OZM V1 NOT INCLUDING HARMONICS
#
This is the repository that constitutes the second experiment with ozm v1 using temporally coupled data. The temporal coupling indicates that there is synchronisation in time between the different measurements of the different meters associated with the common-use synchronisation and the main common meter. In this experiment only the usual measurements are included (therefore we will not use the harmonics of voltage, current and power).

The OZM v1 used to obtain the measurements that we will analyse in this repository is a single-phase electricity meter (although a three-phase version already exists), which is also a power quality analyser. This device is both open source and open hardware, and has been developed jointly by the Universities of Almeria and Granada, and also has IoT capabilities, which not only allows us to measure a wide range of electrical variables at a high sampling frequency of 15625 Hz (voltage, current, active power, reactive power, total harmonic distortion or THD, power factor and harmonics of both current and voltage and power up to the order 50), but also allows us to capture and process all these measurements.

We use 6 OZM v1 counters applied to 5 common household appliances. This is the list of devices:

1- Mains (main meter)
2-Kettle
3-Fan
4-Freezer
5- TV
6-Vacuum cleaner

In this repository we analyse the impact of NOT taking harmonics to disaggregate electricity consumption using OZM v1 and the NILMTK Toolkit. In this repository we specify the number of measurements supported by the different OZMs, such as active, apparent and reactive power, frequency, voltage, current and power factor. We do NOT add the harmonics of voltage (50), current (50) and power (50), i.e. harmonics are ignored.

The contents of the yaml metadata files are attached in a separate directory inside root, as well as the new converter that also disregards harmonics. It should be noted that the Jupyter Notebook contains not only the Python code but also the results of running the DSUALM0H dataset. It should also be noted that to run this code we need to have the NILMTK toolkit installed (also available on Github), as well as the new dataset that already includes the harmonics.

**DUE TO ITS SIZE, THE DATA FILES ARE NOT AVAILABLE IN CSV FORMAT IN THIS SPECIFIC REPOSITORY, BUT THE COMPLETE DATASET WITH ALL THE HARMONICS IS AVAILABLE IN THE DSUALMH REPOSITORY.**DUE TO ITS SIZE, THE DATA FILES ARE NOT AVAILABLE IN CSV FORMAT IN THIS SPECIFIC REPOSITORY, BUT THE COMPLETE DATASET WITH ALL THE HARMONICS IS AVAILABLE IN THE DSUALMH REPOSITORY.**

Note: The DS is over 25MB and has been uploaded compressed with winrar.
Our goal is to provide NILM researchers with new data repositories to expand the existing range. As these new datasets may contain more than 150 electrical variables recorded at high frequency in different everyday applications, by offering this wide range of data, we hope to boost and improve research in the NILM field.
A continuación, pues si más dilación, se expone de forma genérica las conclusiones principales de tomar solo los armónicos impares, despreciando todos los armónicos o tomando todos ellos (pares e impares).

# INTRODUCTION#

Given the current energy crisis, the so-called Non Intrusive Load Monitoring or NILM approach, which consists of estimating the individual consumptions of different electrical appliances connected to a central point from a single smart meter, has come back into focus. In this context we have the NILMTK which provides a complete pipeline of converters, evaluation metrics, algorithms, etc. that lowers the entry barrier for researchers.

On the other hand, the OZM is an advanced electrical energy meter, which is also a power quality analyser. It is open source and open hardware, with IoT capabilities, and can measure a wide range of electrical variables at a high sampling rate of 15625 Hz [17] including voltage, current and power transients up to order 50.Given the current energy crisis, the so-called Non Intrusive Load Monitoring or NILM approach, which consists of estimating the individual consumptions of different electrical appliances connected to a central point from a single smart meter, has come back into focus. In this context we have the NILMTK which provides a complete pipeline of converters, evaluation metrics, algorithms, etc. that lowers the entry barrier for researchers.


![Un ratón de computadora Descripción generada automáticamente con confianza media](media/8b142fc4123b890a4b89929ad749c4f0.png)

Illustration 1-Appearance of the OZM

Precisely thanks to the characteristics of the OZM, the aim of this work is to use the data yielded by the new devices to show the use and potential applications in terms of energy desegregation with this data, adapting the non-intrusive load monitoring tool NILMTK for this purpose. It should be noted that in order to capture OZM data both without using harmonics and using voltage, current and power harmonics up to order 50, as well as to associate the corresponding OZM metadata, two new converters and converters are provided that have allowed us to create two new DS: DSUALM and DSUALMH stored in the HDF5 format..

# RELATED WORK
Existing energy disaggregation methods can be classified into four main groups: **optimisation methods** (highlighting Vector Support Machines or SVM , Bird swarm algorithm or BSA , genetic algorithms and Particle Swarm Optimization or PSO among others), **supervised methods** (highlighting Bayesian classifiers , Support Vector Machine or SVM , the Discriminative Disaggregation Sparse Coding algorithm or DDSC , Artificial Neural Networks or ANN and their extensions), **unsupervised methods** (including Combinatorial Optimisation or CO, Markov Models or HMM and their extensions, such as FHMM ), and **others*.*.

On the other hand there are Public Datasets that can be used to test and compare the results offered by different energy disaggregation algorithms, among which we highlight **AMPds**: readings for a 1' overall meter, as well as sub-metered readings of 19 individual circuits, **DRED:** electricity data, environmental information, occupancy information and household information, **ECO**: 1 Hz aggregate consumption data and also data taken at 1 Hz from selected appliances in 6 households for 8 months, **GREEND**: power data taken at 1 second intervals from 9 appliances and the total energy demand of 9 households over one year, **HES:** measurements of 51 appliances at 2' intervals from 251 households over 12 months, **IAWE**: aggregate measurements and submeasurements of electricity and gas from 33 appliances at 1" resolution over 73 days from one household, **REDD:** power measurements at 3" to 4" intervals from 6 US households, **REDD:** power measurements at 3" to 4" intervals from 6 US households, **REDD**: aggregate measurements and submeasurements of electricity and gas from 33 appliances at 1" resolution over 73 days from one household, **REDD**: aggregate measurements and submeasurements of electricity and gas from 33 appliances at 1" resolution over 73 days from one household, **REDD:** power measurements at 3" to 4" intervals from 6 US households, **REDD**: aggregate measurements and submeasurements of electricity and gas from 6 US households over 12 months. US, **REFIT:** aggregate and 9 individual appliance power measurements from 20 homes, with a resolution of 1 sample every 8"**, UK-DALE:** 16 kHz aggregate measurements and 6" sub-metered energy data from individual appliances in 3 UK homes, as well as 1" aggregate and 6" sub-metering for 2 additional homes, **DEPS:** 1 Hz readings on 6 devices present in a classroom taken over one month.

#  ARCHITECTURE


For the disaggregation process we will use the NILMTK Toolkit, the flow of which can be seen in the following illustration.

![Diagrama Descripción generada automáticamente](media/eded0ee570875a3acd6095e09964fcc8.png)


   llustration 2-Flowchart NILMTK

1.  **Generation of new SDs
The models presented in this paper make use of data from recordings of several hours of operation of different devices, using the OZM API, collecting fundamental and secondary electrical characteristics, such as complex harmonic values of current, voltage and power up to order 50.

![Imagen que contiene interior, tabla, pequeño, cocina Descripción generada automáticamente](media/ee788945b64977eead987834be39d4a0.png)

Illustration 3-Taking measurements with OZM

We have as a container for the measurements collected from the OZMs, files with 160 data fields, fields, by the way, not all of which will be relevant, at least in the first phase of the study, so we need to adapt these to be able to use them in the NILMTK. In a first step, we will make a preliminary analysis of the data files, for which, initially, both the metadata and the data are analysed, whose structure should be based on the NILMTK-DF format. Next, we need new converters where we will make a series of manipulations with the information of the fields provided in the data files in hdfs or csv format, which will lead us to the creation of the final output data files, which we will save in csv format.The following task is the conversion of the different measurement files in csv format pre-treated in the previous phase to a single common file in HDF5 format), and that we store in the folder "/data/" which **will also contain all the metadata of the DS.**.

Normally standardised DS formats are used in NILMTK, but given the exclusivity of the data offered by the OZM, we require a new data format, for which we create two functions: **convert_ualm** and c**onvert_ualmt** (to process the transients).In the directories of the new converters we place, not only the Python code of the new converters, but we also include new subdirectories in "/metadata/", which will include the metadata files in yaml format. We can see in Illustration 4 the configuration of all the necessary files for the converters, as well as the required directory structure.

![Diagrama Descripción generada automáticamente](media/grafico4.png)

Illustration 4-Metadata file structure

As each csv file is obtained in the previous phase from the OZM files, it is necessary to number them, with number 1 corresponding to the main meter. To do this, the new function accesses all the aforementioned measurement data files located in the input folder "/electricity/", using the labels.csv file, a process that is shown in Illustration 5.

![Diagrama Descripción generada automáticamente](media/grafico5.png)

Illustration 5-Data file structure

Once all the measurement files have been processed, we proceed to merge them in yaml format, and then convert the data structure into a new DS in H5 format. Once the data files are located, the first thing to do is to invoke the DS converter by calling the new function **convert_ualm**, passing it the path of the metadata and the new name of the DS file that will be generated in H5 format. Once the new DS is created, we can perform a pre-analysis of the data, being especially interesting to represent the voltage, power and current graphs for the different applications.

![Diagrama Descripción generada automáticamente](media/84316c491442f29646c911888dd9c632.png)

Illustration 6- Representation of measurements

2. **Analysis, Pre-processing, Training, Validation, Disaggregation and Metrics**


Once we have generated the new DSs, we can use the implementations available in NILMTK to perform a quick diagnosis of the DS. It is especially interesting to obtain the voltage profile and to obtain the area graph of the applications (Illustration 7).

![C:\\Users\\carlo\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.MSO\\FA49089.tmp](media/6c530bb7b86df3f629e85148c9324d90.png)

Illustration 7-Voltage profile

It is also interesting to know possible missing sections or to discard those samples with very low values (applying filters). Finally, after analysing the data, we will divide the DS into **training set, validation set and test set.** To train the model, we use two of the disaggregation models available in NILMTK, such as the supervised algorithms CO and FHMM, using the active power signal of the devices. To do this, in addition to loading the necessary libraries, we will first define the DS and associate the labels associated with the appliances, and then we can define the training subset.Once the training model is defined, thanks to the fact that NILMTK implements the two desegregation algorithms, we will run the two algorithms CO and FHMM in these time intervals (10", 30", 60", 5', 10", 15"), for the three methods (First, Mean and Median), saving the models generated in H5 format. It would only be necessary to implement the best evaluated model in the validation stage, and in this way we can compare the real signal (GT) against the one predicted by the best model.

Once the training model is defined, thanks to the fact that NILMTK implements the two desegregation algorithms, we will run the two algorithms CO and FHMM in these time intervals (10", 30", 60", 5', 10", 15"), for the three methods (First, Mean and Median), saving the models generated in H5 format. It would only be necessary to implement the best evaluated model in the validation stage, and in this way we can compare the real signal (GT) against the one predicted by the best model.

![Gráfico, Gráfico circular Descripción generada automáticamente](media/4d0776cb0dcedda7fda16481b8e30dec.png)

Illustration 8-Comparison of GT with actual measurements

As we can see, the results are quite good in terms of predictions, since, for example, for the kettle (in blue) there is only a small deviation of 0.2% from the actual data. Likewise, both the fan (in red) and the light (pink) show a minimal deviation and the hoover (in orange) only shows a deviation of 1.6%.

# RESULTS# 

NILMTK has the calculation of evaluation metrics through the use of the MeterGroup, for the validation of the results by means of the validation set. It is necessary to run different metrics such as FEAC, F1, EAE, MNEAP and RMSE on the models obtained, which gives us an output similar to Table 1.

![Tabla Descripción generada automáticamente](media/297c59ec44c1b7470918f21972f879d2.png)

Table 1-Main metrics obtained for the applications

**F1 and MNEAP metrics**

In terms of F1, the incorporation of transients allows an improvement for the kettle and the hoover, while maintaining for the fan and freezer and worsening for the TV.

![Interfaz de usuario gráfica Descripción generada automáticamente](media/62372595a25be760f9a7edc2816dba0f.png)

Illustration 9-Comparison with or without transients for F1

With respect to MNEAP, the incorporation of harmonics improves the performance for the fan and, notably, for the freezer, maintaining similar values for the rest of the appliances.

![Diagrama, Esquemático Descripción generada automáticamente](media/914bbb75525398421ef19ff9f4f05767.png)

Illustration 10-Comparison with and without transients for MNEAP

**RMSE**

In terms of RMSE, the improvement of the freezer clearly stands out (**from 62.25 to 35.9**) followed by a timid improvement in the TV (from 24.5 to 23.2). Regarding other appliances, the fan remains the same, and both the kettle and the hoover worsen.

![Interfaz de usuario gráfica, Aplicación Descripción generada automáticamente](media/24e3d1a8c9a98785a4854e6a836cc9a7.png)

Table 2-Comparison with RMSE transients

**Summary of results with or without harmonics**.

In general, the incorporation of transients improves all metrics for almost all applications. Particularly noteworthy are the fan and freezer. For the TV it would only worsen for F1 and for the hoover or the kettle it would only worsen for RSMSE.

![Tabla Descripción generada automáticamente](media/becc766374ae392852e175dc3b5f867a.png)

Illustration 11 Summary metrics with or without harmonics

**Comparison with other DSs**

For the IAWE DS the results show that the most efficient algorithm for this DS is the combinatorial (CO) algorithm using the Mean method and **period 10 minutes versus only the 10 seconds needed with the OZM** data.

![Tabla Descripción generada automáticamente](media/8ebd0ec88d9f344dac9a694ecfd873f8.png)

Illustration 13 IAWE results

On the other hand, the results obtained for the DS of DEPS show a better performance for the CO algorithm, Mean method, but **at a sampling time of half an hour compared to only 10 seconds for the OZM data**.

![Tabla Descripción generada automáticamente](media/8a92a55f7a375c6f472b1acb1a9a86a7.png)

Likewise, if we compare GT and Pred for the DS of DEPS , the divergences are very important, ranging between 1.4%, 4.6% and 4.9% compared to 0 and 1.6% for DSUAL.Likewise, if we compare GT and Pred for the DS of DEPS.

![Gráfico, Gráfico circular Descripción generada automáticamente](media/2b62711a9cf58ecc96ca66410a1d8862.png)

Illustration 15-Comparison GT with Pred for DEPS

# CONCLUSIONS# 

In addition to incorporating the metrics and tools available in the NILMTK toolkit, the new 13-digit timestamp format and two new converters for measurements obtained from OZM (with or without transients) as well as two new converters based on IAWE (with or without transients) have been added to this work, thus eliminating the entry barrier for all researchers who have one or more OZM and wish to access NILM.
On the other hand, if we compare the results of the metrics obtained on DSUALM or DSUALMT, compared to IAWE or DEPS, the results are much worse, especially in terms of the sampling period required, with the values obtained for the MNEAP metric standing out.

# Publications# 

There is an article by me about the NILM using single-phase OZM hardware instead of OZM v2:

\- C. Rodriguez-Navarro, A. Alcayde, V. Isanbaev, L. Castro-Santos, A. Filgueira-Vizoso, and F. G. Montoya, “DSUALMH- A new high-resolution dataset for NILM,” \*Renewable Energy and Power Quality Journal\*, vol. 21, no. 1, pp. 238–243, Jul. 2023, doi: 10.24084/repqj21.286.

Also, in order to make all this work replicable, a new open multi-counter called OMPM has been developed and is published in the scientific journal "Inventions".

\- C. Rodríguez-Navarro, F. Portillo, F. Martínez, F. Manzano-Agugliaro, and A. Alcayde, “Development and Application of an Open Power Meter Suitable for NILM,” \*Inventions\*, vol. 9, no. 1, p. 2, Dec. 2023, doi: 10.3390/inventions9010002.
