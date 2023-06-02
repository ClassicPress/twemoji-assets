# twemoji-assets

This repository contains the files served at https://twemoji.classicpress.net

- `11/`: Twemoji version `11.0.0`. Used in ClassicPress 1.0.0
- `12/`: Twemoji version `12.0.1`. Used from ClassicPress 1.0.1
- `14/`: Twemoji version `14.0.2`. Used from ClassicPress 2.0.0


Files are copied from https://github.com/twitter/twemoji/.

**Instructions to update Emoji releases**

1. Clone this repository:

    `git clone https://github.com/ClassicPress/twemoji-assets`

2. Download the latest Twemoji release from: https://github.com/twitter/twemoji/releases

    Unzip the Twemoji release and copy the `72x72` and `svg` folders from assets to a new folder in the ClassicPress repository
    Commit these changes and then perform a `git pull` on the http://twemoji.classicpress.net/ server

3. Find the commit number for the release from Twemoji `gh-pages`, for example for 14.0.2
    https://github.com/twitter/twemoji/tree/gh-pages/v/14.0.2
    In this case `16fb3e0`

    In the ClassicPress API repository:
    https://github.com/ClassicPress/ClassicPress-APIs
    Amend the `twemoji/update.sh` script to use the new commit version and `v` location
    In the example above the line:

    `for spec in 16fb3e0:v/14.0.2/svg; do`

    Commit this change.

4. Conntect via ssh to the `https://api-v1.classicpress.net` server, `git pull` the changes made in step 3 and then run `update.sh` in the `twemoji` folder.

    Check that the json has been created as:
    https://api-v1.classicpress.net/twemoji/
    
    It should take the form of the commit hash, and version with some added underscores and appended with json, in this example `16fb3e0_v_14.0.2_svg.json`

5. In the core code for ClassicPress https://github.com/ClassicPress/ClassicPress-v2/

    Create a new branch and then update the URL for the json file in the build tools
    `tools/build/emoji.js`

    Also update the ClassicPress Twemoji URL links in the following files:
    ```
    src/wp-includes/formatting.php
    tests/phpunit/tests/formatting/Emoji.php
    ```

6. Run `grunt precommit:emoji`
    This command will add the Emoji Unicodes to:
    `src/wp-includes/formatting.php`

    Commit these changes to your local branh and open a PR for review - done!
