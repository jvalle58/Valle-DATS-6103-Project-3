# The COVID-19 Pandemic in the US by the Numbers

Welcome!  You have arrived at analysis of the spread of the novel coronavirus (COVID-19) in the United States in 2020, from its first case on January 22 to November 30. Since the end of 2019, the spread of the COVID-19 from Wuhan, China has brought with it a devastating toll on human lives and economies around the world today. Considering its unique and highly contagious nature compared to other viruses, it should come as no surprise that it continues to dominate worldwide news cycles amid the rush to develop and distribute an effective vaccine and save as many lives possible.

## Preparing our Data

Our data found at the Centers for Disease Control and Prevention (CDC) [here](https://data.cdc.gov/Case-Surveillance/United-States-COVID-19-Cases-and-Deaths-by-State-o/9mfq-cb36). From the get-go, we should be aware that given the current news cycle in the US, the CDC's data is bound to change as we learn more about newly recorded cases and deaths from the virus. The particular data applied for this project accounts for cases and deaths up to November 30.

Before we can proceed with our analysis, we need to make sure we are working with the right pieces to visualize our data later.  In particular, we should be wary about including any areas that are not designated as one of the fifty states.  For example, any major cities inside the US that are already assigned their own rows should be included within their corresponding states for consistency purposes.  In addition, while our data already accounts for every date from January 22 to November 30, it would be prudent for us to further distinguish between the dates by referencing back to their respective months.  If we can accomplish this, we can more easily present our findings in terms of monthly trends.

Click [here](https://raw.githubusercontent.com/jvalle58/Valle-DATS-6103-Project-3/main/covid-nov-30.csv) to access the raw CSV data file applied for this project.

## Process of Analysis

Once we have our data ready for analysis, we will start off with an overview of COVID-19's effect throughout the whole US. We can dissect this further by comparing figures accounting for the whole year with those recorded by month. In this manner, we can visualize any spikes in cases and deaths over time. 

To get a more in-depth look on COVID-19 however, we can go beyond our original inquiry by examining the virus's spread between the individual states. Using this approach, we will isolate which states were hit the hardest over time and deduce which ones may have responded better in dealing with the virus. This latter note can be addressed by observing the virus's fatality rate. For the course of this project, the fatality rate will be defined as the proportion of new deaths over new cases. Later, we will inspect monthly trends between the states, and thereby identify any spikes over the year. Furthermore, we will apply these trends to predict how the states will fare against COVID-19 at the turn of the new year. 

Click [here](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/DATS%206103%20-%20Individual%20Project%203%20-%20Joseph%20Valle.ipynb) for a look at this project's Jupyter Notebook on GitHub. Some code excerpts are provided below.

### Analyzing COVID-19's Spread Throughout the US (Over the Whole Year)

`a = test1.iloc[:,0].plot(color='tab:blue', fontsize=13)`

`a.set_title('New COVID-19 Cases in the US over Time', fontsize=17)`

`a.set_xlabel('Date', fontsize=15)`

`a.set_ylabel('Number of Cases', fontsize=15)`

`fmt1 = '{x:,.0f}' #We want to express figures in the thousands with commas.`

`t1 = mtick.StrMethodFormatter(fmt1)`

`a.yaxis.set_major_formatter(t1)`

`plt.legend(loc='best', fontsize=13)`

`plt.show()`

`#Based on our dataframe, the reason we apply new, not total, cases here is to observe any spikes in cases over time.`

![Figure 1](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%201.png?raw=true)

`b = test1.iloc[:,1].plot(color='tab:orange', fontsize=13)`

`b.set_title('New COVID-19 Deaths in the US over Time', fontsize=17)`

`b.set_xlabel('Date', fontsize=15)`

`b.set_ylabel('Number of Deaths', fontsize=15)`

`b.yaxis.set_major_formatter(t1)`

`plt.legend(loc='best', fontsize=13)`

`plt.show()`

`#Similarly, applying new rather than total deaths to our plot allows us to report on spikes in deaths over time.`

![Figure 2](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%202.png?raw=true)

`c = test1.iloc[:,3].plot(color='tab:red', fontsize=13)`

`c.set_title('Fatality Rate of New COVID-19 Cases in the US over Time', fontsize=17)`

`c.set_xlabel('Date', fontsize=15)`

`c.set_ylabel('Fatality Rate (%)', fontsize=15)`

`plt.legend(loc='best', fontsize=13)`

`plt.show()`

![Figure 3](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%203.png?raw=true)

---

### Analyzing COVID-19's Spread Throughout the US (Over Each Month)

`d = monthly.iloc[:,2].plot(kind='bar', color='tab:purple', fontsize=13)`

`d.set_title('Fatality Rate of New COVID-19 Cases in the US by Month', fontsize=17)`

`d.set_xlabel('Month', fontsize=15)`

`d.set_ylabel('Fatality Rate (%)', fontsize=15)`

`plt.show()`

![Figure 4](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%204.png?raw=true)

---

`def PiePlot1(variable):`

    df = monthly[variable]
    top_months = df.sort_values(ascending=False)
    top_months = top_months.reset_index()
    top_months.index = top_months.index + 1
    other_months = top_months[6:].sum()[1]
    top_months = top_months[:6]
    top_months.loc[7] = ('All Other Months', other_months) 
    
    #Any months not in the top 6 for a given variable are collected together in a separate slice.
    
    MonthPlot = top_months[variable].plot.pie(subplots=True,
                                              autopct='%0.2f%%',
                                              fontsize=12,
                                              figsize=(10,10),
                                              legend=False,
                                              labels=top_months['Month'],
                                              shadow=False,
                                              explode=(0.15,0,0,0,0,0,0), #The largest slice explodes from the rest.
                                              startangle=90)
                                              
`PiePlot1('New Cases')`

`plt.show()` 

![Figure 5](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%205.png?raw=true)

`PiePlot1('New Deaths')`

`plt.show()` 

![Figure 6](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%206.png?raw=true)

---

### Comparing Between the States (Over the Whole Year)

`def PiePlot2(variable):`

    df = test2[variable]    
    top_states = df.sort_values(ascending=False)
    top_states = top_states.reset_index()
    top_states.index = top_states.index + 1
    other_states = top_states[9:].sum()[1]
    top_states = top_states[:9]
    top_states.loc[10] = ('All Other States', other_states) 
 
    #Any states not in the top 9 for a given variable are collected together in a separate slice.
    
    StatesPlot = top_states[variable].plot.pie(subplots=True,   
                                               autopct='%0.2f%%', 
                                               fontsize=12, figsize=(10,10), 
                                               legend=False, 
                                               labels=top_states['State'], 
                                               shadow=False,
                                               explode=(0.15,0,0,0,0,0,0,0,0,0), #The largest slice explodes again.
                                               startangle=90)

`PiePlot2('Total Cases')`

`plt.show()`

![Figure 7](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%207.png?raw=true)

`PiePlot2('Total Deaths')`

`plt.show()`

![Figure 8](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%208.png?raw=true)

---

`def covid_map(variable):`

    fig = go.Figure(data=go.Choropleth(
        locations=test2.index,
        z = test2[variable],
        text = test2.index, #The text will help us identify which state from our map we want to study.
        locationmode = 'USA-states',
        autocolorscale = False, #Varying shades of blue and red will indicate COVID-19's impact throughout the states.
        marker_line_color = 'rgb(255,255,255)',
        marker_line_width = 2,
        colorbar_tickprefix = '',
        colorbar_title = str(variable)+' with COVID-19'
    ))
    
    fig.update_layout(
        title_text = str(variable)+' with COVID-19 Between the States'+'<br>Hover For Value',
        geo = dict(
            showframe = False,
            showcoastlines = True,
            showlakes = True,
            lakecolor = 'rgb(95,145,235)',
            projection_type = 'albers usa'),
        annotations = [dict(
            x = 0.7,
            y = 0.05,
            xref = 'paper',
            yref = 'paper',
            text = 'Source: <a href="https://data.cdc.gov/">CDC</a>',
            showarrow = False
        )]
    )
    
    fig.show() #The resulting map will allow us to hover over any state we want to study.
    
`covid_map('Total Cases')`

![Figure 9](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%209.png?raw=true)

---

`covid_map('Total Deaths')`

![Figure 10](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2010.png?raw=true)

---

`covid_map('Fatality Rate')`

![Figure 11](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2011.png?raw=true)

---

### Predictions: Comparing Between the States (Over Each Month)

`def StatesGraph(state1, state2, variable): #This function will allow us to jointly plot any two states of our choice.`

    df1 = test3.loc[state1, variable] 
    df2 = test3.loc[state2, variable]
    e = df1.plot(color='tab:blue', marker='o', fontsize=13)
    f = df2.plot(color='tab:red', marker='o', fontsize=13)
    
    #The markers help identify each of the monthly data points for both states.
    
    plt.title(str(variable)+' from COVID-19 in '+str(state1)+' and '+str(state2)+' by Month', fontsize=17)
    plt.xlabel('Month', fontsize=15)
    plt.ylabel(str(variable), fontsize=15)
    if variable == 'Fatality Rate':
        plt.ylabel(str(variable)+str(' (%)')) 
        
    #The Y-axis for our Fatality Rate data must be represented in terms of percentages.
    
    if variable != 'Fatality Rate':
        e.yaxis.set_major_formatter(t1) 
        f.yaxis.set_major_formatter(t1)
    
    #For our New Cases and New Deaths data, we want to write any values in the thousands with commas.
        
    state1_patch = mpatches.Patch(color='tab:blue', label=str(state1))
    state2_patch = mpatches.Patch(color='tab:red', label=str(state2))
    plt.legend(title='State', handles=(state1_patch, state2_patch), fontsize=13, loc=(1.02,0))
    plt.show()

`StatesGraph('NY','FL','New Cases')`

![Figure 12](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2012.png?raw=true)

`StatesGraph('NY','FL','New Deaths')`

![Figure 13](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2013.png?raw=true)

`StatesGraph('NY','FL','Fatality Rate')`

![Figure 14](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2014.png?raw=true)

---

`StatesGraph('VA','MD','New Cases')`

![Figure 15](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2015.png?raw=true)

`StatesGraph('VA','MD','New Deaths')`

![Figure 16](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2016.png?raw=true)

`StatesGraph('VA','MD','Fatality Rate')`

![Figure 17](https://github.com/jvalle58/Valle-DATS-6103-Project-3/blob/main/Figure%2017.png?raw=true)

---

## Findings and Conclusions

COVID-19 continues to disturb observers about its potency throughout the US today. In fact, at more than 30%, November reported the highest number of new cases. This unfortunately hints that it is becoming easier for the virus to spread from person to person, even in asymptomatic cases. In addition, April reported the greatest number of new deaths along with the highest fatality rate of all months reported thus far. As an extension to this observation, while February reported the third-highest fatality rate, it should be noted that only 19 cases and 1 death were recorded at that time, both of which were significantly lower than all subsequent months.

That being said, it is encouraging that the fatality rate for new COVID-19 cases has dropped from more than 6% at its peak in April to less than 1% today. Additionally, while spikes in cases have steepened over time, especially in November, they have become relatively less fatal as well. Hence, new cases have far outpaced new deaths recently. As our knowledge of the virus has grown, our treatments for it have also improved with time, dropping the fatality rate. From this, we can predict that new cases will begin to drop substantially by around the summer of next year, when warmer weather and greater distribution of a vaccine will improve herd immunity to the virus.

When looking at COVID-19 from a geographic perspective, it can be deduced that the virus's toll on the US is not consistent or uniform throughout the individual states. All else held equal, it's not surprising to note that states with higher populations tend to report more cases and deaths overall. Besides this observation however, population density is another factor in predicting COVID-19's impact on a state. Taking into account the latest recommendations from the CDC with regard to social distancing, states with higher population densities are more vulnerable to the virus. Looking at it from another angle, urban areas are more likely to witness spikes in cases and deaths than their rural counterparts. As we observed from our heat map covering the whole US, New York State reported more deaths from COVID-19 than any other state thus far. This can be explained by the role of New York City, which became the epicenter of the virus's spread starting in March, and witnessed a high fatality rate. In fact, New York State has accounted for almost 13% of total deaths from the virus since the CDC began recording cases and deaths in January.

While rising case numbers are legitimate causes for concern due to COVID-19's high contagiousness, we should not jump to conclusions that higher case totals are necessarily indicative of higher death totals, or vice versa. This is particularly evident when we apply our last visualization.

Two states that have received significant attention from the media on their COVID-19 responses, New York and Florida, provide an excellent example of this fallacy. While Florida's surge in cases in July was higher than New York's April surge, it actually witnessed fewer deaths over the course of the year. As an extension to this, it was more successful than New York in flattening its curve with respect to new deaths for a prolonged period of time, including July. In contrast, New York's early surge proved significantly more fatal, and while its death total has plateaued since then, it still leads all other states in this category. From this example, if current trends maintain themselves, we can predict that both states' death totals will continue to plateau as herd immunity improves. If cases do rise substantially again however, New York may especially be vulnerable to a more fatal surge, as cold weather arrives and forces more people to stay indoors together for longer periods of time, thus allowing the virus to spread more easily.

As an alternate approach, we can plug in Virginia and Maryland. These states have aroused concerns among observers as of late because cases have risen by more than a third in both since September. In terms of new deaths and the fatality rate, both states peaked in May and April, respectively, with Virginia witnessing another, albeit smaller, spike in deaths in September. As Virginia and Maryland have considerably smaller populations than New York and Florida, we can predict that cases will continue to rise at a faster rate in these states than the latter ones in the coming months. We may also see proportionately more deaths in both, especially around the Washington, DC area.

References for this project are listed below.

## References

Extracting just Month and Year separately from Pandas Datetime column. (2014, August 05). Retrieved November 7, 2020, from https://stackoverflow.com/questions/25146121/extracting-just-month-and-year-separately-from-pandas-datetime-column

How to convert index of a pandas dataframe into a column? (2013, December 09). Retrieved November 20, 2020, from https://stackoverflow.com/questions/20461165/how-to-convert-index-of-a-pandas-dataframe-into-a-column

Pandas: Sum two rows of dataframe without rearranging dataframe? (2016, June 21). Retrieved November 12, 2020, from https://stackoverflow.com/questions/37947479/pandas-sum-two-rows-of-dataframe-without-rearranging-dataframe

United States COVID-19 Cases and Deaths by State over Time. (2020, June 11). Retrieved November 4, 2020, from https://data.cdc.gov/Case-Surveillance/United-States-COVID-19-Cases-and-Deaths-by-State-o/9mfq-cb36

Hunter, J., Dale, D., Firing, E., & Droettboom, M. (Eds.). (n.d.). Matplotlib.patches. Retrieved November 23, 2020, from https://matplotlib.org/3.2.1/api/patches_api.html

Hunter, J., Dale, D., Firing, E., & Droettboom, M. (Eds.). (n.d.). Matplotlib.markers. Retrieved November 23, 2020, from https://matplotlib.org/3.2.1/api/markers_api.html
