# IntentOS policy notes

Rules the trusted policy cites. Headings are stable identifiers: `policies/*.yaml` binds sections by heading, so edit the text, not the titles.

## Frame without holidays

With holidays=None, create_bdew_load_profiles must issue UserWarning "No holidays given" and, with create_dynamic_h0_profile(), equal self.slp.slp_frame.

Evidence recorded at approval:
- test tests/test_elec_slp.py::TestElecSLP::test_frame_without_holidays: def test_frame_without_holidays(self):

## Warning without holidays

add_weekdays2df(self.df) must issue UserWarning "No holidays given" and produce no weekday value of 0.

Evidence recorded at approval:
- test tests/test_tools.py::TestWeekdays::test_warning_without_holidays: def test_warning_without_holidays(self):

## No warning with empty holidays

add_weekdays2df(self.df, holidays=[]) must not issue a warning and must produce no weekday value of 0.

Evidence recorded at approval:
- test tests/test_tools.py::TestWeekdays::test_no_warning_with_empty_holidays: def test_no_warning_with_empty_holidays(self):

## Weekdays no holidays

add_weekdays2df(self.df) must set the first weekday value to 5.

Evidence recorded at approval:
- test tests/test_tools.py::TestWeekdays::test_weekdays_no_holidays: def test_weekdays_no_holidays(self):

## Energy demand zero sum

When Q_Heiz_a is 0, load_curves.loc[:, ("EFH_1", "EFH", "Q_Heiz_TT")] must contain only 0.

Evidence recorded at approval:
- test tests/test_vdi4655.py::TestVDI4655Profiles::test_energy_demand_zero_sum: def test_energy_demand_zero_sum(self, example_houses):

## Negative factors warning

Negative typical day factors must emit UserWarning containing "was negative, see VDI 4655" while get_load_curve_houses returns non-empty load_curves.

_Approved as INV-002; tests: test_inv_002_negative_factors_warning.py._

## Allow retrieving only warm water profile

Retrieving only the warm water profile must be allowed.

_Approved as INV-004; tests: ._

## House Parameters

Each house must provide a unique name, house_type of "EFH" or "MFH", N_Pers up to 12 (relevant for EFH), N_WE up to 40 (relevant for MFH), and Q_Heiz_a.

_Approved as INV-005; tests: ._

## Hardcoded minutes per year to allow leap year

Minutes per year in src/oemof/demand/vdi/regions.py must account for leap year.

_Approved as INV-006; tests: ._

## Static electricity profiles conserve annual demand

Static BDEW electricity profiles must sum to the requested annual energy demand for each selected sector.

_Approved as INV-007; tests: test_inv_007_static_electricity_profiles_conserve_annual_demand.py._

## Electricity power conversion preserves interval energy

Quarter-hourly BDEW electricity power profiles must equal their energy profiles times the conversion factor, which defaults to four.

_Approved as INV-008; tests: test_inv_008_electricity_power_conversion_preserves_interval_en.py._

## Holiday tagging preserves the chosen day category

Supplied holidays must receive weekday category zero, or Sunday category seven when Sunday treatment is enabled, for both date lists and dictionaries.

_Approved as INV-009; tests: test_inv_009_holiday_tagging_preserves_the_chosen_day_category.py._

## BDEW 2025 profiles retain their annual energy baseline

BDEW 2025 profiles must integrate to approximately 1 GWh for a calendar year, within 1% with the tested holiday calendar and 2.5% without holidays, including shifted and consecutive years.

_Approved as INV-010; tests: test_inv_010_bdew_2025_profiles_retain_their_annual_energy_base.py._

## Industrial profiles conserve the requested period demand

Industrial load profiles must integrate to the requested total demand over the supplied period at both hourly and quarter-hourly resolution.

_Approved as INV-011; tests: test_inv_011_industrial_profiles_conserve_the_requested_period_.py._

## Industrial profiles reject incomplete factor dictionaries

Industrial load profile generation must raise ValueError when required day-category or day/night scaling factors are missing.

_Approved as INV-012; tests: test_inv_012_industrial_profiles_reject_incomplete_factor_dicti.py._

## Industrial workday boundaries use daytime factors

Industrial profiles must apply daytime factors at both configured workday boundaries and nighttime factors immediately outside them.

_Approved as INV-013; tests: test_inv_013_industrial_workday_boundaries_use_daytime_factors.py._

## Non-residential heat profiles require building class zero

Non-residential BDEW heat profiles must accept building class zero and reject nonzero building classes with ValueError.

_Approved as INV-014; tests: test_inv_014_non_residential_heat_profiles_require_building_cla.py._

## VDI demand matches each house and energy carrier

VDI residential load curves must sum to each house's specified annual heating, hot-water and electricity demand independently at hourly and quarter-hourly resolution.

_Approved as INV-015; tests: test_inv_015_vdi_demand_matches_each_house_and_energy_carrier.py._

## Summer heating suppression leaves hot water and power available

With summer heating suppression enabled and fixed seasons, VDI profiles must have zero space-heating demand throughout summer while retaining positive summer hot-water and electricity demand.

_Approved as INV-016; tests: test_inv_016_summer_heating_suppression_leaves_hot_water_and_po.py._

## VDI rejects incomplete climate input

Creating a VDI region with missing cloud-coverage data must raise AttributeError indicating that the climate object is incomplete.

_Approved as INV-017; tests: test_inv_017_vdi_rejects_incomplete_climate_input.py._

## VDI temperature thresholds are optional

VDI house profiles without explicit seasonal temperature thresholds must equal profiles with the default summer threshold of 15 degrees Celsius and winter threshold of 5 degrees Celsius.

_Approved as INV-018; tests: test_inv_018_vdi_temperature_thresholds_are_optional.py._

## VDI timestamps label interval starts

VDI load curves must use timestamps at interval starts, covering the requested year from January 1 at midnight through its final interval at the chosen hourly or quarter-hourly resolution.

_Approved as INV-019; tests: test_inv_019_vdi_timestamps_label_interval_starts.py._

## Weather imports reject missing DWD headers

Importing a weather file without the DWD header marker must raise TypeError identifying the missing header.

_Approved as INV-020; tests: test_inv_020_weather_imports_reject_missing_dwd_headers.py._

## BDEW 2025 resampling conserves energy

BDEW 2025 profiles must preserve time-integrated energy within 0.01% when resampled from aligned quarter-hourly intervals to minute or hourly intervals.

_Approved as INV-021; tests: test_inv_021_bdew_2025_resampling_conserves_energy.py._

## BDEW 2025 excerpts retain calendar-based values

A BDEW 2025 profile generated for a quarter-hourly excerpt must equal the matching slice of the full-year profile with the same holiday calendar.

_Approved as INV-022; tests: test_inv_022_bdew_2025_excerpts_retain_calendar_based_values.py._

## Distinguish omitted holidays from an explicitly empty calendar

Load-profile calendar assignment must emit a UserWarning when holidays are omitted, suppress the missing-holidays warning for an explicit empty list, and retain ordinary weekday classifications in both cases.

_Approved as INV-023; tests: test_inv_023_distinguish_omitted_holidays_from_an_explicitly_em.py._

## Static electricity profiles conserve requested annual energy

Static BDEW electricity profiles must sum to each sector's requested annual energy, and their default quarter-hourly power profiles must integrate to that same energy.

_Approved as INV-024; tests: test_inv_024_static_electricity_profiles_conserve_requested_ann.py._

## Electricity profiles include the leap-year intervals

Leap-year BDEW electricity profiles must contain 35,136 quarter-hourly intervals.

_Approved as INV-025; tests: test_inv_025_electricity_profiles_include_the_leap_year_interva.py._

## BDEW 2025 profiles preserve their power and energy interpretation

BDEW 2025 electricity profiles must represent power on a nominal 1 GWh annual scale and preserve integrated energy when resampled between one-minute, quarter-hourly, and hourly intervals.

_Approved as INV-026; tests: test_inv_026_bdew_2025_profiles_preserve_their_power_and_energy.py._

## Industrial power integrates to demand over the supplied period

Industrial power profiles must integrate to the supplied total demand over the supplied time period, including at quarter-hourly resolution.

_Approved as INV-027; tests: test_inv_027_industrial_power_integrates_to_demand_over_the_sup.py._

## Industrial scaling factors must be complete

Industrial load-profile generation must raise ValueError when supplied scaling factors omit a weekday, weekend, or holiday category or its day or night factor.

_Approved as INV-028; tests: test_inv_028_industrial_scaling_factors_must_be_complete.py._

## Industrial holidays can use Sunday factors

Industrial profiles configured to treat holidays as Sundays must apply the factors assigned to Sunday on those holiday dates.

_Approved as INV-029; tests: test_inv_029_industrial_holidays_can_use_sunday_factors.py._

## Warm-water-only profiles exclude the space-heating contribution

BDEW warm-water-only heat profiles must suppress the space-heating sigmoid contribution while retaining the hot-water contribution.

_Approved as INV-030; tests: test_inv_030_warm_water_only_profiles_exclude_the_space_heating.py._

## VDI profiles conserve every house's annual energy demands

VDI 4655 load curves must sum separately to each house's specified annual heating, hot-water, and electricity demand at hourly and quarter-hourly resolutions.

_Approved as INV-032; tests: test_inv_032_vdi_profiles_conserve_every_house_s_annual_energy_.py._

## Zero annual heating demand produces an entirely zero curve

A VDI 4655 house with zero annual heating demand must receive zero heating demand at every timestep.

_Approved as INV-033; tests: test_inv_033_zero_annual_heating_demand_produces_an_entirely_ze.py._

## VDI leap-year profiles include all 366 days

Hourly VDI 4655 profiles for a leap year must contain 8,784 intervals when supplied with 366 days of climate data.

_Approved as INV-034; tests: test_inv_034_vdi_leap_year_profiles_include_all_366_days.py._

## Negative typical-day demand warns without aborting generation

VDI 4655 generation must warn when a computed typical-day electricity or hot-water demand becomes negative and still produce a load curve.

_Approved as INV-035; tests: test_inv_035_negative_typical_day_demand_warns_without_aborting.py._

## Reject invalid DWD test-reference-year region numbers

VDI climate loading must reject DWD TRY region numbers outside 1–15 with ValueError.

_Approved as INV-036; tests: test_inv_036_reject_invalid_dwd_test_reference_year_region_numb.py._

## Summer heating suppression removes summer space-heating demand

VDI 4655 profiles must assign zero space-heating demand to summer days when summer heating suppression is enabled.

_Approved as INV-037; tests: test_inv_037_summer_heating_suppression_removes_summer_space_he.py._

## Omitted VDI temperature limits use the documented defaults

VDI 4655 houses with omitted seasonal temperature limits must produce the same load curves as otherwise identical houses explicitly using a 15°C summer threshold and a 5°C winter threshold.

_Approved as INV-038; tests: test_inv_038_omitted_vdi_temperature_limits_use_the_documented_.py._

## VDI regions accept only supported residential house types

VDI 4655 regions must reject house types other than single-family EFH or multi-family MFH with ValueError.

_Approved as INV-039; tests: test_inv_039_vdi_regions_accept_only_supported_residential_hous.py._
