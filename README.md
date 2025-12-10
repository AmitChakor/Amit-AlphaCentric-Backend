Alpha_XMLTradeProcessing - Readme

1) Prerequisites
- Windows machine
- Visual Studio 2017/2019/2022 with .NET Framework 4.7.2 development workload installed
- (Optional) MSBuild if you prefer command-line builds

2) Open the project
- Open Visual Studio and choose File -> Open -> Project/Solution
- Open the solution file: Alpha_XMLTradeProcessing.csproj in the project folder

3) Configure input file paths (You can download, copy and extract the "Engineer Code Test.zip" folder from the repository into your C drive )
- By default Program.cs uses hard-coded paths:
  Securities file: C:\Engineer Code Test\Securities.xml
  Trades folder:  C:\Engineer Code Test\Test
- Either place your XML files at those paths or edit Program.cs to point to your desired locations.
  - Path variables are near the top of Main() in Program.cs.

4) Verify UnityConfig and App.config
- Ensure UnityConfig registers the implementations for IFileReader, ISecurityService and ITradeService. This is required for dependency resolution at runtime.
- App.config should be valid for your environment (no extra changes required for basic runs).

5) Prepare input files
- Securities.xml should contain <Securities> / <Security> with <BloombergId> elements.
- Trade files should be placed under the trades root folder and match the pattern Trades*.xml (or ensure files exist and XmlFileReader can read them).
- Ensure XML element names/casing match the models (Trade model expects lowercase element names such as <tradingAccount>, <transactionCode>, and <security><code>).

6) Build the solution
- In Visual Studio: Build -> Build Solution
- From command line: run MSBuild against the .csproj (MSBuild path depends on VS version)

7) Run the application
- From Visual Studio: Debug -> Start Without Debugging (Ctrl+F5)
- The console application will:
  - Load securities
  - Discover and read trade files
  - Aggregate trades and compute totals/averages
  - Detect trades referencing invalid securities
  - Print results to console and write Output.txt to C:\Engineer Code Test\Output.txt (path can be changed in Program.cs)

8) Output and results
- Console: Aggregated trades and invalid security summary are displayed
- File: Output.txt (CSV-like) contains aggregated and invalid summary

9) Troubleshooting
- Empty BloombergId in aggregation:
  - Confirm trade XML mapping: Trade.Security.Code must be populated. The model expects <security><code> in trade XML.
  - Confirm securities are loaded and BloombergId keys match trade codes. Consider trimming/normalizing strings.
- ArgumentNullException at dictionary lookup:
  - Ensure SecurityService.LoadSecurities ran before validating trades. IsValidSecurity defends against null/empty keys.
- File I/O errors:
  - Verify permissions and that folders exist. Run Visual Studio as administrator if necessary.

10) Suggested improvements (optional)
- Add logging for per-file errors and progress
- Normalize codes (Trim/ToUpperInvariant) at load time
- Add unit tests for services using mocks
- For scale: consider XmlReader streaming and controlled parallel processing

11) Contact / Notes
- This project targets .NET Framework 4.7.2. If migrating to .NET Core/.NET 5+, consider switching to Microsoft.Extensions.DependencyInjection for DI and update project files accordingly.

End of Readme
