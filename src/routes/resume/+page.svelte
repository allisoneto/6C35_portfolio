<svelte:head>
  <title>Resume</title>
</svelte:head>


<nav>
    <ul>
        <li><a href=".">Home</a></li>
        <li><a href="projects">Projects</a></li>
        <li><a class="current" href="resume">About</a></li>
        <li><a href="contact">Contact</a></li>
        <li><a href="https://github.com/allisoneto" target="_blank">GitHub</a></li>
    </ul>
</nav>


<header>
    <h1>Allison Eto: Resume</h1>

    <div class="contact-info">
        <ul>
            <li>Honolulu, HI</li>
            <li>(808) 285-2083</li>
            <li><a href="mailto:allisons-backpack@mit.edu" target="_blank">allisons-backpack@mit.edu</a></li>
            <li><a href="https://linkedin.com/in/allison-eto/" target="_blank">LinkedIn</a></li>
        </ul>
    </div>
</header>

<section class="resume-section">
    <h2>Education</h2>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3><strong>Massachusetts Institute of Technology</strong> - Cambridge, MA</h3>
            <p>
                <u>Coursework:</u> Programming Fundamentals, Algorithms, Probability, Machine Learning, Linear Algebra &
                Optimization, Signals & Systems, Embedded Machine Learning, Representation, Inference, and Reasoning in AI,
                Intro Computer Vision, Principles of Automatic Control, Natural Language Processing
            </p>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2023-09">Sep 2023</time> - <time datetime="2027-06">Jun 2027</time></p>
            <p><em>Candidate for B.S. in Computer Science</em></p>
            <p>GPA: 4.8</p>
        </div>
    </div>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3><strong>'Iolani School (HS)</strong> - Honolulu, HI</h3>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2017-09">Sep 2017</time> - <time datetime="2023-06">Jun 2023</time></p>
            <p>GPA: 4.48 (Valedictorian)</p>
        </div>
    </div>
</section>

<section class="resume-section">
    <h2>Experience</h2>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3>Kavli Institute for Astrophysics and Space Research; MIT</h3>
            <ul>
                <li>Develop multi-modal, self-supervised machine learning model to classify variable stars for astronomy research</li>
                <li>Experiment with contrastive and generative techniques, transformer architectures, and hyperparameter choice</li>
            </ul>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2025-05">May 2025</time> - Present</p>
            <p><em>Undergraduate Machine Learning Researcher</em></p>
        </div>
    </div>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3>University of California Observatories</h3>
            <ul>
                <li>Designed and built an open-source data processing Python package for Lick Observatory, creating the first
                    standardized photometric pipeline to streamline user operations on the Nickel Telescope.</li>
                <li>Performed statistical analysis on the Nickel Telescope's main science camera to improve image quality.</li>
            </ul>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2024-06">Jun 2024</time> - <time datetime="2024-08">Aug 2024</time></p>
            <p><em>Software Engineering Intern</em></p>
        </div>
    </div>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3>Astrodynamics, Space Robotics, and Controls Lab (ARCLab); MIT</h3>
            <ul>
                <li>Worked on the COSTARS project team to create very low-cost satellite tracking technology for implementation
                    by the amateur astronomy/educational community.</li>
                <li>Developed computer vision software to detect satellite streaks in low-resolution images & model their orbits.</li>
            </ul>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2024-02">Feb 2024</time> - <time datetime="2024-05">May 2024</time></p>
            <p><em>Undergraduate Software Researcher</em></p>
        </div>
    </div>
</section>

<section class="resume-section">
    <h2>Awards</h2>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <p>US Physics Olympiad Bronze Medal - Top 170 nationwide</p>
        </div>
        <div class="resume-entry-meta">
            <p>2023</p>
        </div>
    </div>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <p>AIME Qualifier - Top ~3% of 90,000 nationwide</p>
        </div>
        <div class="resume-entry-meta">
            <p>2019 - 2023</p>
        </div>
    </div>
</section>

<section class="resume-section">
    <h2>Activities & Leadership</h2>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3>MIT Science Olympiad Tournament</h3>
            <ul>
                <li>Organize logistics and communications for over 1000 students in a 23-event science competition.</li>
            </ul>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2023-09">Sep 2023</time> - Present</p>
            <p><em>Head of Competitor Logistics</em></p>
        </div>
    </div>

    <div class="resume-entry">
        <div class="resume-entry-main">
            <h3>NASA Challenge Teams at MIT</h3>
            <ul>
                <li><u>BIG Idea Challenge 2024</u>: developed a proposal for inflatable water tank architecture.</li>
                <li><u>RASC-AL Challenge 2025</u>: utilizing systems engineering principles to research & design a Mars CubeSat
                    constellation with scientific remote sensing capabilities.</li>
                <li><u>Lunar Autonomy Challenge 2025</u>: designing an autonomous agent to navigate a simulated robotic lunar rover
                    using computer vision & machine learning techniques.</li>
            </ul>
        </div>
        <div class="resume-entry-meta">
            <p><time datetime="2023-09">Sep 2023</time> - <time datetime="2025-06">Jun 2025</time></p>
            <p><em>Team Member</em></p>
        </div>
    </div>

    <div class="resume-entry resume-entry--no-meta">
        <div class="resume-entry-main">
            <h3>Projects</h3>
            <ul>
                <li>Developed a machine learning model to automatically segment mouse kidneys in MRI images</li>
                <li>Designed, built, & coded a digital scale from scratch w/ FSR's, Arduino, C++, & MatLab.</li>
                <li>Designed & iteratively built a multi-stage mechanical timer, winning 6th in a national competition.</li>
            </ul>
        </div>
    </div>
</section>

<section class="resume-section">
    <h2>Skills & Interests</h2>
    <div class="resume-entry resume-entry--no-meta">
        <div class="resume-entry-main">
            <ul>
                <li><u>Languages</u>: Python, Java, Julia, C++, HTML, SQL</li>
                <li><u>Software</u>: Git/GitHub, Windows, Linux, Pandas, Pytorch, Lightning, Arduino, MatLab</li>
                <li><u>Interpersonal</u>: Spanish, written communication, team coordination</li>
            </ul>
        </div>
    </div>
</section>