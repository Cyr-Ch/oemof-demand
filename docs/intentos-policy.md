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

