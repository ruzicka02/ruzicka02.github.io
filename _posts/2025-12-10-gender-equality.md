---
layout: post
title: "Economical and political aspects of gender equality"
---

> Created for the CS-E4450 - Explorative Information Visualization course at
> Aalto University.

<p>
    DISCLAIMER: Due to how the Export to HTML feature works in the Python
    library Plotly, this blogpost only contains videos of the interactive
    visualizations. To see the full interactive versions, please visit the <a
    href="/genderequality2025/">dedicated page</a>. Works best on a desktop
    computer.
</p>

<p>
    This website was created as a school project, together with a <a
    href="/genderequality2025/report.pdf" target="_blank">PDF report</a>, that
    describes the problematic in greater detail. The goal of the project was to
    dive into a new problematic, collect and process data from serious sources,
    and create a set of visualizations that answer our key theses clearly.
</p>

<p>
    I believe that these issues are a topic for the entire society, as the
    entire society would benefit from addressing these issues, and they
    should therefore be treated as such. In addition, this topic is still
    very visible even in our developed “first world”, where hunger or lack
    of clean water may be hard to relate for us.

    Description of the project, motivation, data sources, and goals.
</p>

<h2>Key theses</h2>

<p>
    My approach to this information visualization was to first ask the most
    elementary and crucial questions:
</p>

<ol>
    <li>Are these gender equality issues decreasing over time?</li>
    <li>Is there any significant difference between different countries or
    regions, and does it correlate for example with GDP per capita or the
    Human Development Index?</li>
</ol>

<p>
    After analyzing the mentioned datasets, I found a positive trend of change in
    the past decade or so, in essentially all indicators I observed. This message is
    also carried in the attached visualizations. It is very subjective to say,
    whether the change is sufficient or not, and there are also many factors that
    will determine the future rate of change, which can both increase or decrease.
</p>

<p>
    For some indicators, such as the representation of women in parliaments, there
    seemed to be a significant correlation between rich and equal countries. It
    would be very beneficial to do a proper correlation analysis with another
    metric, as my assumption was rather based on visual understanding. On the other
    hand, the wage gap did not seem to correlate with the standard separation of
    rich and poor countries, at least within the context of the EU.
</p>

<p>
    Specifically, the Eurostat dataset <a
    href="https://ec.europa.eu/eurostat/databrowser/view/earn_gr_gpgr2/default">earn_gr_gpgr2</a>
    contains gender wage gap data for all EU countries excluding Greece, for
    every year between 2014 and 2023. It shows how the EU27 average has
    decreased from 15.7 % to 12 % in these 9 years. The dataset <a
    href="https://ec.europa.eu/eurostat/databrowser/view/sdg_05_50/default">sdg_05_50</a>,
    also from Eurostat, then reports on share of women in national
    parliaments. Containing data from 2003 to 2024, it reports an increase
    from 21.1 % to 33.4 % of women.
</p>

<!-- overview -->
<img src="/img/genderequality2025/overview.webp" alt="Overview of gender
equality data" style="max-width: 100%; height: auto;" />

<p>
    Look at how different countries perform.  Notice how higher salaries do
    not necessarily correspond to more gender equality in earnings. For
    example, Poland has a relatively low average salary but the gender pay
    gap is lower than for example in Germany.
</p>

<!-- earn_gr_gpgr2 -->
<img src="/img/genderequality2025/earn_gr_gpgr2.webp" alt="Gender pay gap in
unadjusted form" style="max-width: 100%; height: auto;" />

<p>
    With the parliamentary seats, the general trend is that the Nordic
    countries (Sweden, Finland, Denmark) perform best, while the Southern
    and Eastern European countries (Greece, Bulgaria, Romania) perform
    worst. There is probably some correlation to be found with economical
    indicators, such as GDP per capita.
</p>

<!-- sdg_05_50 -->
<img src="/img/genderequality2025/sdg_05_50.webp" alt="Seats held by women in
national parliaments" style="max-width: 100%; height: auto;" />

<h2>Data sources</h2>

<p>
    Many Europe-specific statistics were collected by Eurostat, many of
    those are linked on the page <a
    href="https://ec.europa.eu/eurostat/statistics-explained/index.php?title=Gender_statistics">Statistics
    Explained - Gender statistics</a>. In addition, the European Union has a
    dedicated <a href="https://eige.europa.eu/">European Institute for
    Gender Equality</a>, providing its own independent statistics such as
    the <a href="https://eige.europa.eu/gender-equality-index/2024">Gender
    Equality Index</a>.
</p>

<p>
    The following data sources were used to create the visualizations on
    this page:
</p>

<ul>
    <li><a
    href="https://ec.europa.eu/eurostat/databrowser/view/earn_gr_gpgr2/default">Eurostat
    earn_gr_gpgr2</a></li>
    <li><a
    href="https://ec.europa.eu/eurostat/databrowser/view/sdg_05_50/default">Eurostat
    sdg_05_50</a></li>
</ul>
