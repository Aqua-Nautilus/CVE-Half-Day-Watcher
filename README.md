# CVE Half-Day Watcher

CVE Half-Day Watcher is a security tool designed to highlight the risk of early exposure of Common Vulnerabilities and Exposures (CVEs) in the public domain. It leverages the National Vulnerability Database (NVD) API to identify recently published CVEs with GitHub references before an official patch is released. By doing so, CVE Half-Day Watcher aims to underscore the window of opportunity for attackers to "harvest" this information and develop exploits.
This tool is a proof of concept, ready to be built upon and extended.

## What is a "Half-Day" Vulnerability?

* A vulnerability that is known to the party or parties responsible for patching or fixing it. Alarmingly, this vulnerability is exposed on some public platforms such as GitHub commits/PRs/issues, NVD, etc.
* A patch may have been created in the open-source, but the official release is not yet available.
* What is the risk? These kinds of vulnerabilities may be exposed on public platforms (such as NVD, GitHub, etc.), making it possible for attackers to harvest them, locate the vulnerable code, and even write an exploit.
An example: imagine a case where there is an open issue on GitHub about “unwanted” behavior, and a commit that fixes the vulnerable code exists and refers to the issue, but the latest release on the GitHub project does not include the commit that resolves the issue.

![Log4Shell_Timeline.png](/misc/Log4Shell_Timeline.png)

More information can be found on our [blog](https://blog.aquasec.com/50-shades-of-vulnerabilities-uncovering-flaws-in-open-source-vulnerability-disclosures).

## How It Works

CVE Half-Day Watcher scans the NVD for newly pushed CVEs and checks for any GitHub references such as commits, pull requests (PRs), or issues linked to these CVEs. It then verifies if the commit/PR has been included in a release on GitHub (currently for issues it skips this check). If a release including the fix is not available, it flags the CVE to indicate a possible "half-day" vulnerability scenario, where the vulnerability is known but not yet patched.

The tool also scans specified GitHub repositories for suspicious PRs and issues that addressing security vulnerabilties using word-matching algorithm and OpenAI for validation. This feature aims to alert maintainers early about potential security risks that could impact their codebase.

## Installation

Before you begin, ensure you have Python installed on your system. Then, clone the repository and install the dependencies:

```bash
git clone https://github.com/Aqua-Nautilus/CVE-Half-Day-Watcher.git
cd CVE-Half-Day-Watcher
pip install -r requirements.txt
```

## Usage

The CVE Half-Day Watcher can be used in two modes: scanning newly published CVEs from the NVD and scanning specific repositories for potentially vulnerable content.

### NVD Scan Mode

This mode scans the National Vulnerability Database (NVD) for newly published CVEs with GitHub references.

```bash
python scan.py --github_token YOUR_GITHUB_TOKEN --mode feed [--days DAYS] [--min_stars MIN_STARS]
```

* `--github_token`: Your GitHub token for authentication (required).
* `--mode`: Specify the mode to operate in (`feed` for NVD scan).
* `--days`: The number of days to look back for CVEs (optional - default is 3).
* `--min_stars`: The minimum number of stars a repository should have to be considered (optional - default is 150).

An example of the results from November 2, 2023.
![5_half_day_2_x2.png](/misc/5_half_day_2_x2.png)

### Repository Scan Mode

This mode scans titles and bodies of open PRs and issues in specified GitHub repositories using a suspicious word-matching algorithm.

```bash
python scan.py --github_token YOUR_GITHUB_TOKEN --mode repo --repo_full_name YOUR_REPO_NAME
```

* `--github_token`: Your GitHub token for authentication (required).
* `--mode`: Specify the mode to operate in (`repo` for repository scan).
* `--repo_full_name`: Your GitHub repo name in the format: `<Organization name/Repository Name>`. For example: `aqua-Nautilus/CVE-Half-Day-Watcher` (required).
* `--openai_token`: You OpenAI API key(optional)

### Interactive Mode

You can also use the tool in an interactive mode that offers a menu to choose different scanning options.

```bash
python scan.py --github_token YOUR_GITHUB_TOKEN
```

Follow the prompts to choose between scanning by the latest NVD feed or scanning a specific repository.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.