# FPS (Federal Public Services) information

This information is for an FPS that wants to use the BOSA FTS as a signing solution for visitors on the FPS website.

## Documentation

[BOSA signature solution.docx](BOSA%20signature%20solution.docx)

## Error strings

These are the error strings returned in the callback from the BOSA FTS to the FPS (the contents of the 'err' query parameter)

[error_constants.txt](error_constants.txt)

## Sample code

A simple java web service that shows the calls that the FPS should do.
It contains a config.txt file in which the account info for the BOSA S3 service has to be filled in.

[git](https://git-fsf.services.belgium.be/eidas/test-environment/-/tree/master/mintest/test_fps)
