---
layout: article
permalink: /about
titles:
  # @start locale config
  en      : &EN       About Me
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  zh-Hans : &ZH_HANS  关于我
  zh      : *ZH_HANS
  zh-CN   : *ZH_HANS
  zh-SG   : *ZH_HANS
  zh-Hant : &ZH_HANT  關於
  zh-TW   : *ZH_HANT
  zh-HK   : *ZH_HANT
  ko      : &KO       소개
  ko-KR   : *KO
  fr      : &FR       À propos
  fr-BE   : *FR
  fr-CA   : *FR
  fr-CH   : *FR
  fr-FR   : *FR
  fr-LU   : *FR
  # @end locale config
key: page-about
comment: false
---

<div class="item">
    <div class="item__image">
        <img class="image image--lg" src="https://z3.ax1x.com/2021/05/29/2AZNhn.jpg"/>
    </div>
    <div class="item__content">
        <div class="item__header">
            <h2>Haolin Jia</h2>
        </div>
        <div class="item__description">
            <p>
                I am a master student at <a href="https://cims.nyu.edu/">Courant Institute of Mathematical Sciences</a>, New York University, majoring in computer science. I got my Bachelor's Degree of Computer Science and Technology from <a href="https://see-en.tongji.edu.cn/">Tongji University</a> in July 2020.I have been working with <a href="http://cs.nyu.edu/~panozzo/">Prof. Daniele Panozzo</a> on Broad-Phase Detection in the field of geometry modeling. I also did research on video reconstruction and semi-supervised learning with <a href="https://scholar.google.com/citations?user=p9-ohHsAAAAJ&hl=en">Prof. Ming-Hsuan Yang</a>. Currently, I have great interest in Computer Graphics, Geometry Modeling and relative programming which is being learned by myself. Here is my complete <a href="https://drive.google.com/file/d/1hhmqyf-4bAKm3hIHycszZIMoCnyN2_eK/preview" target="blank">cv</a>.
            </p>
        </div>
    </div>
</div>

### Education

<div class="timeline-box">
    <ul id="first-list">
        <li>
            <span></span>
            <div class="title">New York University</div>
            <div class="info">MS in Computer Science</div>
            <div class="name">- New York, NY, U.S. -</div>
            <div class="time">
                <span>Present</span>
                <span>Sep 2020</span>
            </div>
        </li>
        <li>
            <span></span>
            <div class="title">University of California, Merced</div>
            <div class="info">Research Intern supervised by Prof. Ming-Hsuan Yang</div>
            <div class="name">- Merced, CA, U.S. -</div>
            <div class="time">
                <span>Feb 2020</span>
                <span>Jul 2019</span>
            </div>
        </li>
        <li>
            <span></span>
            <div class="title">Tongji University</div>
            <div class="info">BA in Computer Science GPA: 91.4/100</div>
            <div class="name">- Shanghai, China -</div>
            <div class="time">
                <span>Jul 2020</span>
                <span>Sep 2016</span>
            </div>
        </li>
        <li>
            <span></span>
            <div class="title">The Experimental High School Attached To Beijing Normal University</div>
            <div class="info">Senior high school in Class One instructed by Jinnan Yuan</div>
            <div class="name">- Beijing, China -</div>
            <div class="time">
                <span>Jul 2016</span>
                <span>Sep 2013</span>
            </div>
        </li>
    </ul>
</div>

### Publication

<div class="publication-list">
    <div class="grid">
        <div class="cell cell--4">
            <img src="https://z3.ax1x.com/2021/05/31/2Z4idJ.jpg" />
        </div>
        <div class="cell cell--8">
            <div class="title">
                Semi-Supervised Learning with Meta-Gradient
            </div>
            <a class="author" href="https://github.com/Sakura03">Xinyu Zhang</a>,
            <a class="author" href="https://prinsphield.github.io/">Tiahong Xiao</a>,
            <a class="author" href="https://Harrypotterrrr.github.io"><b>Haolin Jia</b></a>,
            <a class="author" href="https://mmcheng.net/cmm/">Mingming Cheng</a>,
            <a class="author" href="https://faculty.ucmerced.edu/mhyang/">Ming-Hsuan Yang</a>,
            <div class="conference">
            International Conference on Artificial Intelligence and Statistics(AISTATS), 2021
            </div>
            [<a class="source" href="https://arxiv.org/abs/2007.03966">Paper</a>,
            <a class="source" href="https://github.com/Sakura03/SemiMeta">Source code</a>,
            <a class="source" href="https://github.com/Sakura03/SemiMeta/blob/master/images/aistats-poster.pdf">Poster</a>,
            <a class="source" href="https://github.com/Sakura03/SemiMeta/blob/master/images/poster-video.mp4">Video</a>,
            <a class="source" href="https://github.com/Sakura03/SemiMeta/blob/master/images/brief-slides.pdf">Short slides</a>,
            <a class="source" href="https://github.com/Sakura03/SemiMeta/blob/master/images/full-slides.pdf">Full slides</a>]
        </div>
    </div>

</div>

### Project

<div class="project-list">
    <div class="grid">
        <div class="cell cell--4">
            <img src="https://github.com/My-code-works/SmartTab/raw/master/model.png" />
        </div>
        <div class="cell cell--8">
            <div class="title">
                SmartTab: Sequential Model for Analysing Structure of Articles
            </div>
            <div class="brief-intro">
            Modeled the analysing structures of articles as a Sequence Labeling problem in 2 steps: Paragraph Embedding and Sequence Labeling. Used BERT as a framework of feature extractor to encode paragraphs to gain each sentences embedding. Tagged each paragraph according to its position within the section it belongs to and the relation between it with the neighbor above.
            </div>
            <div class="position">
            Google, Beijing, China, 2019
            </div>
            [<a class="source" href="https://github.com/My-code-works">Source code</a>,
            <a class="source" href="https://drive.google.com/file/d/17WciUpA0c61phd6JMkPzrcKES9jgR5UG/view?usp=sharing">Poster</a>,
            <a class="source" href="https://drive.google.com/file/d/1N3nnttlhuF3_MV_tAN_vnk8DRsPT3suP/view?usp=sharing">Slide</a>]
        </div>
    </div>
    <div class="grid">
        <div class="cell cell--4">
            <img src="https://z3.ax1x.com/2021/05/31/2ekwFJ.jpg" />
        </div>
        <div class="cell cell--8">
            <div class="title">
                Real-time system of Steam Market Monitor and Database
            </div>
            <div class="brief-intro">
            Building a database by crawling real-time data from the steam trading market. Designing a data analysis algorithm to predict the tendency of the price based on a structured theoretical financial model mixing with Bayesian estimation and even approaches like a temporal sequential network. Maintaining a front-end website to monitor interested goods and items.
            </div>
            <div class="position">
            Beijing, China, 2020
            </div>
        </div>
    </div>

</div>


<!-- <iframe src="https://drive.google.com/file/d/1hhmqyf-4bAKm3hIHycszZIMoCnyN2_eK/preview" width="100%" height="1000"></iframe> -->

<!-- <iframe src="http://docs.google.com/gview?url=https://drive.google.com/file/d/1hhmqyf-4bAKm3hIHycszZIMoCnyN2_eK/view?usp=sharing&embedded=true" style="width:600px; height:500px;" frameborder="0"></iframe> -->
