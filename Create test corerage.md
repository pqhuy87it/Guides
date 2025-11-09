# 1. Install ```lcov```
Open terminal
```bash
brew install lcov
```
With lcov installed, we’ll be set to handle coverage reports like a pro

# 2. Running Flutter Test Coverage
The next step is running tests with coverage enabled. Use this command
```bash
flutter test --no-test-assets --coverage
```
--no-test-assets:  unless your tests specifically need images, fonts, or other assets, adding this will make your test runs faster.
others:
```bash
flutter test --coverage
```

# 3. Cleaning Up the Coverage Report
To view the report, you can use the following command:
```bash
lcov -r coverage/lcov.info
```
If you want to exclude generated files, test utilities, or any irrelevant files from cluttering your report, you can add input like this
```bash
lcov -r coverage/lcov.info "**/base*.dart" "*constants.dart" "*constant.dart" "*/__test*__/*" "*/__mock*__/*" "lib/**/**.g.dart" "lib/**.realm_schema.dart" -o coverage/lcov_cleaned.info
```
1. -r: Tells lcov to remove specified files or paths from the report.
2. Exclusions: Paths like "**/base*.dart", "*/__test*__/*", and "lib/**/**.g.dart" ensure generated or test-related files don’t count in our coverage.

# 4. Generating an HTML Report
Want to see your coverage results in a nice, readable format? Run this:
```bash
genhtml -o /Users/phamhuy/Downloads/test_coverage coverage/lcov.info > coverage/output.txt
```
And this applies if you’re using the cleaned-up version.
```bash
genhtml -o /Users/phamhuy/Downloads/test_coverage coverage/lcov_cleaned.info > coverage/output.txt
```

# 5. Integrating It with CI/CD
First, Let’s not get caught off guard by a drop in coverage. Here’s a script snippet that checks if our coverage meets the minimum threshold:
```bash
# Get the coverage percentage from the report
COVERAGE=$(grep lines coverage/output.txt | awk -F ' ' '{print $2}' | cut -d'.' -f 1)

# Convert to an integer
COVERAGE=$(($COVERAGE+0))

# Set a baseline for minimum coverage
MIN_COVERAGE_VALUE=$1

# Check if current coverage is lower than the baseline
OVERALL_MIN_COVERAGE=$([ $COVERAGE -le $MIN_COVERAGE_VALUE ] && echo $COVERAGE || echo $MIN_COVERAGE_VALUE)

echo "Overall Minimum Coverage: $OVERALL_MIN_COVERAGE%"
```
This script grabs the current coverage, compares it with our minimum threshold, and updates the overall result.
Here is an example of .gitlab-ci.yml
```bash
test-coverage:
  image: ghcr.io/cirruslabs/flutter:latest  # Pick the Flutter version you like
  stage: test
  before_script:
    # Prep the environment
    - flutter pub global activate junitreport
    - export PATH="$PATH:$HOME/.pub-cache/bin"
    - flutter --version  # Just for logging, nice to know what version we’re using
    - OVERALL_MIN_COVERAGE=100  # Start with a high number for comparison

  script:
    # Get dependencies and run tests with coverage
    - flutter pub get
    - flutter test --no-test-assets --coverage

    # Filter out the noise in the coverage report
    - lcov -r coverage/lcov.info "*/__test*__/*" "*/__mock*__/*" "lib/**/**.g.dart" "lib/**.realm_schema.dart" -o coverage/lcov_cleaned.info

    # Generate the HTML report
    - genhtml -o coverage coverage/lcov_cleaned.info > coverage/output.txt

    # Extract and validate coverage percentage
    - COVERAGE=$(grep lines coverage/output.txt | awk -F ' ' '{print $2}' | cut -d'.' -f 1)
    - COVERAGE=$(($COVERAGE+0))
    - MIN_COVERAGE=80  # Adjust this to your project’s minimum requirement

    # Calculate overall minimum coverage
    - OVERALL_MIN_COVERAGE=$(sh ../scripts/calculate-min-coverage.sh $MIN_COVERAGE)

    # Print results and check if we’re above the threshold
    - echo "Configured minimum coverage threshold: $MIN_COVERAGE%"
    - echo "Final overall coverage: $OVERALL_MIN_COVERAGE%"
    - if [ $OVERALL_MIN_COVERAGE -lt $MIN_COVERAGE ]; then
        echo "Coverage is below the minimum threshold—let’s beef up those tests!";
        exit 1;
      else
        echo "Coverage looks great: $OVERALL_MIN_COVERAGE%";
      fi

  interruptible: true  # Stop the job if a newer one starts
  artifacts:
    when: on_failure  # Save coverage artifacts only if the job fails
    expire_in: 1 day  # Keep artifacts for 1 day
    paths:
      - coverage/*  # Include the coverage report

  only:
    - merge_requests  # Run on merge requests only
  tags:
    - runner-tag  # Replace with your runner setup
```
