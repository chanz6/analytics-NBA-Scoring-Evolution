<a id="readme-top"></a>

<!-- PROJECT LOGO -->

  <h3 align="center">NBA Scoring Evolution Analysis</h3>

  <p align="center">
    An analysis that details how scoring in the NBA has evolved
    <br />
    <a href="https://github.com/chanz6"><strong>More Personal Projects »</strong></a>
    <br />
  </p>
</div>

[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#scraped--analyzed-with">Scraped & Analyzed With</a></li>
      </ul>
    </li>
    <li>
      <a href="#project-summary">Project Summary</a>
      <ul>
        <li><a href="#cloning">Cloning</a></li>
        <li><a href="#findings-summary">Findings Summary</a></li>
        <li><a href="#visualizations">Visualizations</a></li>
      </ul>
    </li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->
## About The Project

This project analyzes the evolution of NBA scoring efficiency over the past decade using various data analysis techniques. Upon scraping and analyzing player statistics from the NBA's official stats website, we explore key trends that have influenced the leagues offensive improvement. This project utilizes Python libraries such as pandas, NumPy, Matplotlib, and Seaborn to process, clean, and visualize data. The workflow for this project includes:

- **Data Collection** – Scraping NBA player statistics from [the NBA's official stats website](nba.com/stats) across multiple seasons.
- **Data Cleaning** – Processing raw data, removing inconsistencies, and standardizing team names. 
- **Exploratory Analysis** – Investigating trends in scoring, three-point shooting, and field goal efficiency.
- **Statistical Insights** – Identifying correlations between field goal attempts, three-pointers, and total points scored.
- **Visualization** – Presenting insights using charts, scatter plots, and line graphs to highlight scoring trends.

The findings show how the NBA's offensive game has evolved, with a strong emphasis on three-point shooting as a key driver of increased scoring efficiency. This project provides a deeper look into the strategic shift in professional basketball, backed by rigorous analysis.

### Scraped & Analyzed With

* ![Python][Python]
* ![Pandas][Pandas]
* ![Numpy][Numpy]
* ![Matplotlib][Matplotlib]
* ![Seaborn][Seaborn]

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Project Summary
### Cloning

This project can be cloned using the following command:

```
git clone https://github.com/chanz6/analytics-NBA-Scoring-Evolution.git
```

### Findings Summary
- _Data scraping scipts can be found in the `stats scraping.ipynb` notebook._

- _A more detailed analysis can be found in the `cleaning and analysis.ipynb` notebook._

The data shows that the NBA’s offensive efficiency has been rapidly improving, but this improvement isn't primarily due to better shooting accuracy. Instead, the main driver is the significant increase in three-point attempts. Teams are taking far more shots from beyond the arc, which leads to more three-pointers being made, even if the shooting percentage hasn’t consistently improved. This strategic shift toward a higher volume of three-point shots is the key factor behind the rise in total points scored and the overall boost in offensive efficiency across the league.

### Visualizations

Evolution of PTS (points scored) by season:

![PTS by Season](/images/capture13.PNG)

Evolution of FGM (field goals made) by season:

![FGM by Season](/images/capture14.PNG)

Evolution of 3PM (three-pointers made) by season:

![3PM by Season](/images/capture15.PNG)

Evolution of 3PA (three-pointers attempted) by season:

![3PA by Season](/images/capture16.PNG)

Relationship between 3PM (three-pointers made) and 3PA (three-pointers attempted):

![3PM vs 3PA](/images/capture17.PNG)

Relationship between PTS (points scored) and 3PM (three-pointers made)

![PTS vs 3PM](/images/capture18.PNG)

Evolution of 3P% (three-point percentage) by season:

![3P% by season](/images/capture19.PNG)

Relationship between 3P% (three-point percentage) and PTS (points scored)

![3P% vs PTS](/images/capture20.PNG)


[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=0077B5
[linkedin-url]: https://www.linkedin.com/in/zachary-chann/
[Python]: https://img.shields.io/badge/python-000000?style=for-the-badge&logo=python&logoColor=blue
[Pandas]: https://img.shields.io/badge/Pandas-000bff?style=for-the-badge&logo=pandas&logoColor=purple
[Numpy]: https://img.shields.io/badge/NumPy-ad526f?style=for-the-badge&logo=NumPy&logoColor=blue
[Matplotlib]: https://img.shields.io/badge/Matplotlib-DD0031?style=for-the-badge&logo=matplotlib&logoColor=white
[Seaborn]: https://img.shields.io/badge/Seaborn-4A4A55?style=for-the-badge&logo=seaborn&logoColor=FF3E00
