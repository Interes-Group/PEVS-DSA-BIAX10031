# PANEUROUNI Dátové štruktúry a algoritmy - BIAX10031 course

![Hugo 0.143.1](https://img.shields.io/badge/Powered%20by%20Hugo-0.143.1-ff4088)
![GitHub License](https://img.shields.io/github/license/interes-group/PEVS-DSA-BIAX10031)
[![Deploy site to GitHub Pages](https://github.com/Interes-Group/PEVS-DSA-BIAX10031/actions/workflows/gh-pages.yaml/badge.svg)](https://github.com/Interes-Group/PEVS-DSA-BIAX10031/actions/workflows/gh-pages.yaml)

This repository hosts the website for the university course **"Dátové štruktúry a algoritmy"** (Data Structures and
Algorithms) for the 2024/2025 summer semester. Built with [Hugo](https://gohugo.io/)
and [Hextra theme](https://github.com/imfing/hextra), this site provides course
materials, announcements, and additional resources to support both students and instructors.

## Features

- **Course Materials:** Syllabus, lecture notes, assignments, and additional resources.
- **Announcements:** Stay updated with the latest course-related news.
- **Responsive Design:** Optimized for both desktop and mobile viewing.
- **Static Site Generator:** Powered by Hugo with Hextra theme for fast, secure, and easy-to-maintain content.

## Prerequisites

- **Hugo:** Version 0.143 or higher is recommended.  
  Installation instructions can be found on the [Hugo website](https://gohugo.io/getting-started/installing/).
- **Git:** Basic knowledge of Git is necessary to clone the repository and contribute.

## Getting Started

### Clone the Repository

Open your terminal and run:

```bash
git clone https://github.com/Interes-Group/PEVS-DSA-BIAX10031.git
cd PEVS-DSA-BIAX10031
```

### Local Development

1. **Install Hugo:**  
   Follow the [official installation guide](https://gohugo.io/getting-started/installing/).

2. **Start the Hugo Server:**

   ```bash
   hugo server --buildDrafts --disableFastRender
   ```

   Open your browser and visit [http://localhost:1313](http://localhost:1313) to preview the website.

### Building the Site

To generate the static site files, run:

```bash
hugo --gc --minify
```

The static site will be output to the `public` directory, ready for deployment.

## Usage

This repository is structured to make updating course content straightforward. You can modify existing pages, add new
content, or adjust the layout and styling according to the needs of the course. All changes will automatically update
the site when the Hugo server is running.

Content is structured based on part of the course. All learning material is in content folder with these sub-folders

- **assignments** - Term assignments to students to work during the term. These should be evaluated with points value.
- **examples** - Practical examples to further explain course topics.
- **exercises** - Task and exercises to more practice course topics for students.
- **lectures** - Recording and materials of lectures.
- **other** - Other materials and manual that could help students in their study.

## Contributing

Contributions to improve this course website are welcome. Please follow these guidelines:

1. **Fork the repository.**
2. **Create a feature branch:**

   ```bash
   git checkout -b feature/my-feature
   ```

3. **Commit your changes:**

   ```bash
   git commit -m "Description of your feature"
   ```

4. **Push to your fork:**

   ```bash
   git push origin feature/my-feature
   ```

5. **Open a Pull Request** detailing the changes and improvements.

For any changes or suggestions, please open an issue first to discuss your ideas.

## Code of Conduct

This project adheres to a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you agree to uphold these standards.

## License

This project is licensed under the [Apache-2.0 License](LICENSE).

## Contact

For any questions, issues, or suggestions, please open an issue on GitHub or reach out directly to the repository
maintainers.

---

Happy learning and coding!
