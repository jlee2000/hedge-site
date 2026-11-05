Medicare Advantage medical loss ratio of the four national payers, 2014 to 2023
File: ma_mlr_four_payers_2014_2023.csv
Hedge Quantitative Research LLC, hedgequantresearch.com
Built 2026-11-04.

WHAT THE TABLE IS

One row per payer and contract year, 40 rows: the Medicare Advantage medical loss ratio (MLR) of
UnitedHealth Group, Humana, CVS Health (Aetna) and Elevance Health (Anthem) for contract years 2014
to 2023, built from CMS's public files and nothing else. The chart in the explainer "How to use the
CMS Medicare Advantage MLR public use files" is drawn from this table: ratio_dollar_basis for 2014
to 2017 and 2023, ratio_enrollment_basis for 2018 to 2022.

METHOD: TWO REGIMES

CMS publishes contract dollars for 2014 to 2017 and for 2023. For those years each payer's ratio is
(claims plus quality improvement) divided by (revenue minus taxes and fees), summed over the payer's
contracts. For 2018 to 2022 the public files carry only CMS's adjusted MLR, a different measure, so
those years form a separate series and are not joined to the dollar years.

For 2018 to 2022 CMS publishes no dollars, only each contract's credibility-adjusted MLR, so each
payer's figure for those years is an average of its contracts' ratios, weighted by each contract's
December enrollment as CMS's Monthly Enrollment by Contract file prints it. That file gives one
total per contract. The county-level enrollment file does not: it blanks every county cell of 10 or
fewer enrollees and lists every county in a plan's service area, so most blanked cells are empty.
Measured against the contract totals, a blanked cell holds fewer than 0.2 enrollees on average for
three of the four payers and 0.5 to 1.1 for the fourth, and the blanked cells together hold between
0.6 and 1.6 percent of a payer's enrollment. Treating them as zero gives the same series to within
0.15 points; treating each as 5 moves one payer by up to 2.8 points.

For a contract that offers Part D, CMS's MLR combines its Part C and Part D experience, and so do
the dollar sums here.

COLUMNS

payer: UnitedHealth Group, Humana, CVS Health (Aetna) or Elevance Health (Anthem)
year: contract year, 2014 to 2023
dollars_available: True where CMS publishes contract dollars (2014 to 2017, 2023), else False
weighting_regime: premium (dollar years, ratio of summed dollars) or enrollment (2018 to 2022)
ratio_basis: benefit_ratio_target (dollar years) or adjusted_mlr_enr_wt (2018 to 2022)
n_contracts: the payer's contracts in the cell, after the exclusions below
parent_year_ratio: the cell's ratio: whichever of the next two columns is filled
ratio_dollar_basis: mlr_numerator / mlr_denominator; dollar years only
ratio_enrollment_basis: CMS adjusted MLR, mean weighted by December enrollment; 2018 to 2022
total_revenue: total revenue, dollars, summed over the payer's contracts; dollar years only
member_months: member months, summed over the payer's contracts; dollar years only
incurred_claims: incurred claims, dollars, summed; dollar years only
qia_expense: quality-improvement spend, dollars, summed; dollar years only
taxes_and_fees: taxes and fees, dollars, summed; dollar years only
mlr_numerator: claims plus quality-improvement spend, dollars, summed; dollar years only
mlr_denominator: revenue net of taxes and fees, dollars, summed; dollar years only
cms_published_numerator_sum: numerator on CMS's published definition; equals mlr_numerator
cms_published_denominator_sum: denominator on CMS's published definition; equals mlr_denominator
cms_published_ratio: cms_published_numerator_sum / cms_published_denominator_sum
claims_over_gross_revenue: incurred_claims / total_revenue; dollar years only
weight_total: member months (dollar years) or summed December enrollment (2018 to 2022)
weight_n_fallback: contracts with no December total that took an imputed weight; 0 in every row
weight_fallback: whether any contract took an imputed weight; none in every row

An empty cell is a column that does not apply to the row's regime. Ratios are decimals to six
places, dollars are US dollars to the cent, counts are integers. UTF-8, LF line ends, one header
row.

EXCLUSIONS

- Standalone Part D contracts (contract numbers beginning S or E whose plans are all prescription
  drug plans) are netted out of every payer.
- Cost contracts are netted by the organization type the year's December CPSC file gives them
  (section 1876 or section 1833 cost), because their rows in the MLR file carry the contract's Part
  D offering only. This removes 45 contract-years over the ten years, all section 1876 cost
  contracts. One of them sat inside a payer series (UnitedHealth Group 2017), and taking it out
  moves that year by 0.0003 points.
- CMS leaves non-credible contracts (MA under 2,400 member months, Part D under 4,800) and
  single-plan contracts out of the public files. The contracts left out hold 1.5 to 3.7 percent of
  MA and MA-PD December enrollment in each year, and 0.0 to 6.6 percent of the four payers' own.

ATTRIBUTION RULE

Each contract-year goes to a payer by the Parent Organization string in CMS's December CPSC file
(Monthly Enrollment by Contract/Plan/State/County) of the same year. The string is lower-cased,
stripped of punctuation and trailing corporate suffixes, and matched by six prefix rules anchored at
its start, never by a loose substring: humana (Humana), unitedhealth group (UnitedHealth Group), cvs
health and aetna (CVS Health), elevance health and anthem (Elevance Health). A string that matches
Elevance Health or Anthem but also bundles independent Blue Cross Blue Shield plans goes to review
and stays out of the Elevance series; one contract does so in each year. A contract missing from its
own year's December takes its parent from the nearest other December that lists it, ties going to
the earlier; five 2019 contract-years resolve this way, all from December 2018. No contract-year is
assigned by hand: where a December string names a subsidiary rather than its listed parent, the
table follows the string as CMS published it. CVS Health includes Aetna in every year, including
before the November 2018 acquisition; Elevance Health includes Anthem.

SOURCES

CMS's Medicare Advantage and Part D medical loss ratio page:
https://www.cms.gov/medicare/health-drug-plans/medical-loss-ratio

1. Part C and Part D Medical Loss Ratio public use files, contract years 2014 to 2023, with the
sha256 of each ZIP as downloaded for this table (the live files matched on 2026-11-04). CMS replaced
the CY2019, CY2020 and CY2023 files after first posting them; these are the replacements (see
puf_release_dates.csv).

   CY2014  https://www.cms.gov/medicare/medicare-advantage/plan-payment/downloads/cy2014-mlr-data-release-file.zip
          sha256 c56481823d5d7919ed9a8190c6f005ee514bef2d7f4f338cf162074a486e447c
   CY2015  https://www.cms.gov/files/zip/cy-2015-mlr-data-release-file.zip
          sha256 f79c3934c12967af643d3ef5eb4ed437d90036599e07c28d49816fc5af6c4514
   CY2016  https://www.cms.gov/files/zip/cy-2016-mlr-data-release-file.zip
          sha256 8fc303af84fc0b1b55cc563be1a7c7ff27da5e6e119917b96014d2bf2d90141c
   CY2017  https://www.cms.gov/files/zip/cy-2017-mlr-data-release-file.zip
          sha256 1465e494dd2fb01a257dbae7718564c886a79cec7931877b82a9415a5bcbfd5e
   CY2018  https://www.cms.gov/files/zip/cy-2018-mlr-data-release-file.zip
          sha256 bd40b8a1362a10dd0a3deed16db46adb5d4f48f284d53b66f1378aabe7eaf2b5
   CY2019  https://www.cms.gov/files/zip/cy-2019-mlr-data-release-file.zip-0
          sha256 a32a27ed5e6e5254665d172ee35e50f014d7fac174ab1de91054efdd15d57103
   CY2020  https://www.cms.gov/files/zip/cy-2020-mlr-data-release-file.zip-0
          sha256 8e3ff1659d32aa32b8e2d3975ab19b10c5d31f951ffd5037993ec45b6d987f8a
   CY2021  https://www.cms.gov/files/zip/cy-2021-mlr-data-release-file.zip
          sha256 8fb103ff0af01195322ecc23654e16f8b0f8defef6670d2d366b0d7173a904ff
   CY2022  https://www.cms.gov/files/zip/cy-2022-mlr-data-release-file.zip
          sha256 e7b2e85130ddb1d94b0f7f1436c780c94e3d5d13a515e260b63ee3b2a83f9bb0
   CY2023  https://www.cms.gov/files/zip/public-use-file-cy-2023-zip.zip
          sha256 ea254dfbbd7ea27a37fde8f603eb9dd1ae40e8349aef6d828a4e89a95cd25e8e

2. December Monthly Enrollment by Contract/Plan/State/County (CPSC) files, 2014 to 2023: the Parent
Organization string and the organization type. sha256 of each ZIP as downloaded.

   Dec 2014  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2014/dec/cpsc-enrollment-2014-12.zip
            sha256 78905d90330b6d87e107e99880bed37319bdb9673c75ecda8c77601eaa89db00
   Dec 2015  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2015/dec/cpsc-enrollment-2015-12.zip
            sha256 fe14d45c9efebbb52c76c59bd3b48e0fb604e6717f73f87b7c0df3f6f56ad145
   Dec 2016  https://www.cms.gov/files/zip/cpscenrollment201612zip
            sha256 a1791d7406835b41163dac2c5c68fb69f23b87e04f503cbcb950c3d5fd2dd2d7
   Dec 2017  https://downloads.cms.gov/files/CPSC_Enrollment_2017_12.zip
            sha256 716cd87965270447226dc631c7b703ebe0088adb1b3cc157921d6d6814f7a2bb
   Dec 2018  https://downloads.cms.gov/files/CPSC_Enrollment_2018_12.zip
            sha256 47278239f677b380c5cf4ed24e7ec75fe74806b52771243d8f696b6488d77bbc
   Dec 2019  https://www.cms.gov/files/zip/monthly-enrollment-cpsc-december-2019.zip
            sha256 bb00afdaf4bf5f103200f2a9047e58d209313798de96c3f3800c52a497c7d70c
   Dec 2020  https://www.cms.gov/files/zip/monthly-enrollment-cpsc-december-2020.zip
            sha256 bddb9c1afbaddcd654424fd481360e72b796ab2d47e5318ab15ff076694c6970
   Dec 2021  https://www.cms.gov/files/zip/monthly-enrollment-cpsc-december-2021.zip
            sha256 42b7d1421594a0961ec30296577f2a6dded731ee67760d7746d8bb076ea510c2
   Dec 2022  https://www.cms.gov/files/zip/monthly-enrollment-cpsc-december-2022.zip
            sha256 c1f4b04786310c2a15cc7834838d84e84801683584258d66b8ac2009bdeb1607
   Dec 2023  https://www.cms.gov/files/zip/monthly-enrollment-cpsc-december-2023.zip
            sha256 df39b5fd457c72540875bf8e35e549c6adf4029059e20b508b245dd3fe5cf2be

3. December Monthly Enrollment by Contract files, 2014 to 2023: the 2018 to 2022 weights. sha256 of
the CSV inside each ZIP (Monthly_Report_By_Contract_YYYY_12.csv).

   Dec 2014  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2014/dec/monthly-report-by-contract-2014-12.zip
            sha256 7694de45ed4ee4f21ed6d99fa3ab9fdc93b30edba9ed8149048f772d55652141
   Dec 2015  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2015/dec/monthly-report-by-contract-2015-12.zip
            sha256 dce047f7990ad16a168efa39be2309a79df2e5247c85c5347306b085d96b1024
   Dec 2016  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2016/dec/monthly-report-by-contract-2016-12.zip
            sha256 44141c917cd326f5e1546866e2bd7e5bc05ba0b94e7d34bf0c3ab513cd7d91f6
   Dec 2017  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2017/dec/monthly-report-by-contract-2017-12.zip
            sha256 3476f008612fd23bfab46d2679c48c81244be280d61a0515973be9499d248137
   Dec 2018  https://www.cms.gov/research-statistics-data-and-systems/statistics-trends-and-reports/mcradvpartdenroldata/downloads/2018/dec/monthly-report-by-contract-2018-12.zip
            sha256 4fe6ed4b11eade7f39c33f859e6ad5be52ada4ae21dbb01fd1a2180295ecf271
   Dec 2019  https://www.cms.gov/files/zip/monthly-enrollment-contract-december-2019.zip
            sha256 3dc9633d4ef2f835a54320a7f6c633e1f53ae1e28b2439badb8229f11c0f836b
   Dec 2020  https://www.cms.gov/files/zip/monthly-enrollment-contract-december-2020.zip
            sha256 4744fc8c30e09067ad8ef21c7c0338a0c1403651541ac21e9ecf74bb0aa5801b
   Dec 2021  https://www.cms.gov/files/zip/monthly-enrollment-contract-december-2021.zip
            sha256 ad68e1eee2bf5ef6524c66abe4ed7ea65ae2538e4e004d876cef89e411bb2e15
   Dec 2022  https://www.cms.gov/files/zip/monthly-enrollment-contract-december-2022.zip
            sha256 b638e2226b08706d35b09e2d606ebca436e7837c6a82ba77c8a7ddb7c0b9b34e
   Dec 2023  https://www.cms.gov/files/zip/monthly-enrollment-contract-december-2023.zip
            sha256 4bbe0c907ad4dd8cd56394710f7ff0907dc966f11354e491c8b3e2133ecd7d1f

COMPANION FILE

puf_release_dates.csv: when CMS posted each of the ten MLR public use files, one row per contract
year. The posting window runs from the last archived capture of CMS's MLR page without the year's
file to the first capture with it (both capture URLs are in the row); months are counted from 31
December of the contract year; last_modified_header is the server's Last-Modified header on the live
file, which for CY2019, CY2020 and CY2023 dates the replacement, not the first posting.

LICENSE

Data: CC BY 4.0, Hedge Quantitative Research LLC. Underlying CMS files are United States government
works. License text: https://creativecommons.org/licenses/by/4.0/

Hedge Quantitative Research LLC, hedgequantresearch.com
