Drupal is not as common as WordPress, leaving less of a demand for Drupal scanners; therefore, those that are created often end up abandoned. That is not to say that old scanning applications are no longer of use; they can still quickly detect version numbers and any old plugins that may be vulnerable.

This lab will address two automated scanners: **Droopescan** and **Drupwn**.

### Droopescan
Droopescan is the oldest of the two scanners and receives occasional updates. It has also been updated to scan other CMS platforms like WordPress and Joomla. There are not many options for this scanner:

- `scan` will run a series of automated scans to look for known modules and themes. It performs several thousand checks in total and can take several minutes to complete.
```bash
droopescan scan drupal -u http://<TARGET_IP>
```

- `stats` shows which versions exist for each CMS and how many checks are made.
```bash
droopescan stats
```

### Drupwn
Drupwn is a relatively new scanner that cannot perform as many module detection searches as droopescan. However, it does have several other useful features like ‘User Detection’, ‘Default File Detection’ and a CVE scanner or exploit module; the latter is still a work in progress.
```bash
python3 drupwn enum http://<TARGET_IP>
```

`python3 drupwn -h` allows you to see what options are available for a scan. For example, if you only wanted to carry out user detection you would do so with the following command:
```bash
python3 drupwn enum http://<TARGET_IP> --users
```

It is also important to note that there are new vulnerabilities that might not exist in the scanning application, so using it as a definitive check is not recommended.

### Note
You will need to run Drupwn from the directory where the file is located. This can be done by running `cd drupwn` before entering the example command shown above.