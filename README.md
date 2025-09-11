# MatMef
Matlab wrapper for MEF 3.0 library

This project provides Matlab (mex) functions to read and write MEF 3.0 data using the MEF 3.0 library

## Using pre-compiled mex files
1. Clone or download (and extract) the matmef repository
2. Use the functions (as illustrated in the examples below)

## Building from source
1. Clone the matmef repository using: `git clone https://github.com/MultimodalNeuroimagingLab/matmef.git`
2. Start matlab and set the matmef folder as your working directory
3. To compile the .mex files, run the following lines in matlab:

   - `mex read_mef_session_metadata.c matmef_mapping.c mex_utils.c matmef_dataconverter.c`
   - `mex read_mef_ts_data.c matmef_read.c mex_utils.c matmef_dataconverter.c`
   - `mex init_mef_struct.c matmef_mapping.c mex_utils.c matmef_dataconverter.c`
   - `mex write_mef_segment_metadata.c matmef_write.c mex_utils.c matmef_utils.c matmef_mapping.c matmef_dataconverter.c`
   - `mex write_mef_ts_segment_data.c matmef_write.c mex_utils.c matmef_utils.c matmef_mapping.c matmef_dataconverter.c`

Additional requirements for MAC users prior to mex compilation above:

   a) install xcode Open Mac App Store → Search for Xcode → Install.
   b) in terminal, switch to full Xcode developer directory: sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
   
   c) accept the license: sudo xcodebuild -license press (to display license): enter type: agree
   
   d) verify MATLAB detects the compiler (back in matlab): mex -setup C --> You should now see an option like: Xcode with Clang
   
   e) compile .mex files (see step 3 above)

## Matlab usage examples
```
%  
% High-level  
%  

% metadata only  
[metadata] = readMef3('./mefSessionData/');

% two channels  
[metadata, signaldata] = readMef3('./mefSessionData/', [], {'Ch02', 'Ch07'});  

% all channels, samples 0-1000  
[metadata, signaldata] = readMef3('./mefSessionData/', [], [], 'samples', 0, 1000);  

% two channels, samples 0-1000  
[metadata, signaldata] = readMef3('./mefSessionData/', [], {'Ch02', 'Ch07'}, 'samples', 0, 1000);  

% all channels, multiple ranges/epochs
ranges = [[0,    1000]; ...
          [1000, 2000]; ...
          [5000, 6000]];
[metadata, signaldata] = readMef3('./mefSessionData/', [], [], 'samples', ranges);
```


```
%  
% Low-level  
%  
  
session = read_mef_session_metadata('./mefSessionData/', [], 1);  
data = read_mef_ts_data('./mefSessionData/channelPath/');  
data = read_mef_ts_data('./mefSessionData/channelPath/', [], 'samples', int64(0), int64(1000));
data = read_mef_ts_data('./mefSessionData/channelPath/', [], 'time', int64(1578715810000000), int64(1578715832000000));
```

## Acknowledgements

- Written by Max van den Boom (Multimodal Neuroimaging Lab, Mayo Clinic, Rochester MN)
- Adapted from PyMef (by Jan Cimbalnik, Matt Stead, Ben Brinkmann, and Dan Crepeau)
- EEGLAB plugin code by Arnaud Delorme
- Contributors: Richard J. Cui, Dan Crepeau

- This project was funded by the National Institute Of Mental Health of the National Institutes of Health Award Number R01MH122258 to Dora Hermes
