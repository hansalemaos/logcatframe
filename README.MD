# Class for capturing and parsing Android device logs using ADB.

## pip install logcatframe

### Tested against Windows / Python 3.11 / Anaconda


```python

    Args:
    - adb_path (str): Path to the ADB executable.
    - device_serial (str): Serial number of the target Android device.
    - print_output (bool): Flag to control whether to print log output to the console.
    - su (bool): Flag indicating whether to use 'su' (superuser) for logcat command (default is True).
    - clear_logcat (bool): Flag indicating whether to clear existing logcat logs before starting (default is True).

    Methods:
    - __init__(self, adb_path, device_serial, print_output, su=True, clear_logcat=True):
        Initializes a LogCatFrame instance.

    - start_recording(self):
        Starts capturing Android device logs using the specified ADB path and device serial.
        If `clear_logcat` is True, it clears existing logcat logs before starting.
        The captured logs are stored in the instance's `alldata` attribute.

    - _read_stdout(self, pr):
        Internal method for reading and printing logcat output.
        Used by the `start_recording` method in a separate thread.

    - parse_all_data(self, as_pandas=False):
        Parses the captured logs into a list of dictionaries.
        If `as_pandas` is True, converts the list into a Pandas DataFrame (requires Pandas to be installed).

    - parse_activities(self):
        Parses executed activities from the captured logs.
        Returns a list of tuples containing the executed command and associated variables.

    - get_short_path_name(long_name):
        Returns the short path name for the given long file name.
        Only applicable on Windows; on other platforms, it returns the input unchanged.


    from logcatframe import LogCatFrame

    adblog = LogCatFrame(
        adb_path=r"C:\Android\android-sdk\platform-tools\adb.exe",
        device_serial="emulator-5554",
        print_output=True,
        su=True,
        clear_logcat=True,
    )
    adblog.start_recording()
    df=adblog.parse_all_data(as_pandas=True)
    listoflist=adblog.parse_all_data(as_pandas=False)
    activities=adblog.parse_activities()
    from PrettyColorPrinter import add_printer # optional
    add_printer(1)
    print(df)
    print(listoflist)
    print(activities)


    # [('am start -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -f 0x10200000 -n com.android.settings/.Settings -b [47,217][161,363]',
    #   [['act', 'android.intent.action.MAIN'],
    #    ['cat', '[android.intent.category.LAUNCHER]'],
    #    ['flg', '0x10200000'],
    #    ['cmp', 'com.android.settings/.Settings'],
    #    ['bnds', '[47,217][161,363] (has extras)']]),
    #  ('am start -f 0x8000 -n com.android.settings/.Settings$PowerUsageSummaryActivity',
    #   [['flg', '0x8000'],
    #    ['cmp',
    #     'com.android.settings/.Settings$PowerUsageSummaryActivity (has extras)']]),
    #  ('am start -f 0x8000 -n com.android.settings/.Settings$DisplaySettingsActivity',
    #   [['flg', '0x8000'],
    #    ['cmp',
    #     'com.android.settings/.Settings$DisplaySettingsActivity (has extras)']]),
    #  ('am start -a com.android.intent.action.SHOW_BRIGHTNESS_DIALOG -n com.android.systemui/.settings.BrightnessDialog',
    #   [['act', 'com.android.intent.action.SHOW_BRIGHTNESS_DIALOG'],
    #    ['cmp', 'com.android.systemui/.settings.BrightnessDialog']]),
    #  ('am start -f 0x8000 -n com.android.settings/.Settings$SystemDashboardActivity',
    #   [['flg', '0x8000'],
    #    ['cmp',
    #     'com.android.settings/.Settings$SystemDashboardActivity (has extras)']]),
    #  ('am start -a android.intent.action.MAIN -n com.android.settings/.SubSettings',
    #   [['act', 'android.intent.action.MAIN'],
    #    ['cmp', 'com.android.settings/.SubSettings (has extras)']])]
```