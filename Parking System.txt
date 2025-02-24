%% Parking Managment System 
% Version 1.0.0
% 1st Edition 
% By : Mobin Ghavidel
% HAVE FUN !
%% Data Readings
data_parking_spots_raw = fileread('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt');
data_parking_spots = textscan(data_parking_spots_raw, '%s', 'Delimiter', '\n');
lines_parking_spots = data_parking_spots{1};
data_cars_raw = fileread('C:\Users\Homelan\Desktop\ParkingSystem\cars.txt');
data_cars = textscan(data_cars_raw, '%s', 'Delimiter', '\n');
lines_cars = data_cars{1};
data_prices_raw = fileread('C:\Users\Homelan\Desktop\ParkingSystem\prices.txt');
data_prices = textscan(data_prices_raw, '%s', 'Delimiter', '\n');
lines_prices = data_prices{1};
choice = 0;
%% Car Check in
while true
      % Main menu for check-in
    car_check_in = menu('What Is Your Car Model?', 'Car1(small)', 'Car2(medium)', 'Car3(large)', 'Exit');
    
    if car_check_in == 4 % Exit option
      time6 = msgbox('Goodbye! Have a great day!');
        uiwait(time6)
        break
    end
    
switch car_check_in
    case 1
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'small' size and 'empty' status
    filteredData = data(strcmp(data.size, 'small') & strcmp(data.status, 'empty'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:},'Find Me The Best Option For me');
    
    switch spotIndex
        case numel(options) + 1

 % Define the options
options1 = {'I Want The Most Affordable Spot', 'I Want The Most Secure Spot', 'I Want My Spot To Not Get Dirty','Never Mind'};
numOptions1 = numel(options1);

% Create the figure
Quality = uifigure('Name', 'What Do You Expect From your Parking Spot?', 'Position', [100, 100, 300, 50 + 30 * numOptions1]);

% Create a ButtonGroup to enforce single selection
buttonGroup1 = uibuttongroup(Quality, ...
    'Position', [10, 40, 280, 30 * numOptions1], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions1
    uiradiobutton(buttonGroup1, ...
        'Text', options1{i}, ...
        'Position', [10, 30 * (numOptions1 - i), 250, 22]);
end

% Add "OK" button
okButton1 = uibutton(Quality, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(Quality));

% Wait for user interaction
uiwait(Quality);

% Get the selected option
selectedOption1 = buttonGroup1.SelectedObject.Text;

% Close the figure
close(Quality);

% Display the selected option
disp('Selected Option:');
disp(selectedOption1);
switch selectedOption1
    case 'I Want The Most Affordable Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'small' size and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'small' size and 'empty' status
     filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'small'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case 'I Want The Most Secure Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'small' size and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'small' size and 'empty' status
     filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'small'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case  'I Want My Spot To Not Get Dirty'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'small' size and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'small' size and 'empty' status
     filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'small'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'small'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    end
    break
end
end
    otherwise
        time5 = msgbox('Alright,Whatever Costumer Says! 😊');
        uiwait(time5)
        % Ask if the user wants to check in another car
    another_check_in = menu('Do you want to check in another car?', 'Yes', 'No');
    
    if another_check_in == 2 % If the user chooses No, exit the loop
        time1 = msgbox('Thank you for using the parking system!');
        uiwait(time1);
        break;
    end
end
    end
    % Handle the user's selection
    if spotIndex > 0 && choice == 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Display the selected spot
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'small'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
        break

 case 2
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'medium' size and 'empty' status
    filteredData = data(strcmp(data.size, 'medium') & strcmp(data.status, 'empty'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:},'Find Me The Best Option For me');
    
    switch spotIndex
        case numel(options) + 1

 % Define the options
options1 = {'I Want The Most Affordable Spot', 'I Want The Most Secure Spot', 'I Want My Spot To Not Get Dirty','Never Mind'};
numOptions1 = numel(options1);

% Create the figure
Quality = uifigure('Name', 'What Do You Expect From your Parking Spot?', 'Position', [100, 100, 300, 50 + 30 * numOptions1]);

% Create a ButtonGroup to enforce single selection
buttonGroup1 = uibuttongroup(Quality, ...
    'Position', [10, 40, 280, 30 * numOptions1], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions1
    uiradiobutton(buttonGroup1, ...
        'Text', options1{i}, ...
        'Position', [10, 30 * (numOptions1 - i), 250, 22]);
end

% Add "OK" button
okButton1 = uibutton(Quality, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(Quality));

% Wait for user interaction
uiwait(Quality);

% Get the selected option
selectedOption1 = buttonGroup1.SelectedObject.Text;

% Close the figure
close(Quality);

% Display the selected option
disp('Selected Option:');
disp(selectedOption1);
switch selectedOption1
    case 'I Want The Most Affordable Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'medium' size and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'medium' size and 'empty' status
     filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'medium'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case 'I Want The Most Secure Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'medium' size and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'medium' size and 'empty' status
     filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'medium'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case  'I Want My Spot To Not Get Dirty'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'medium' size and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'medium' size and 'empty' status
     filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'medium'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'medium'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    end
    break
end
end
    otherwise
        time5 = msgbox('Alright,Whatever Costumer Says! 😊');
        uiwait(time5)
        % Ask if the user wants to check in another car
    another_check_in = menu('Do you want to check in another car?', 'Yes', 'No');
    
    if another_check_in == 2 % If the user chooses No, exit the loop
        time1 = msgbox('Thank you for using the parking system!');
        uiwait(time1);
        break;
    end
end
    end
    % Handle the user's selection
    if spotIndex > 0 && choice == 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Display the selected spot
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'medium'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
        break
        
         case 3
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'large' size and 'empty' status
    filteredData = data(strcmp(data.size, 'large') & strcmp(data.status, 'empty'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:},'Find Me The Best Option For me');
    
    switch spotIndex
        case numel(options) + 1

 % Define the options
options1 = {'I Want The Most Affordable Spot', 'I Want The Most Secure Spot', 'I Want My Spot To Not Get Dirty','Never Mind'};
numOptions1 = numel(options1);

% Create the figure
Quality = uifigure('Name', 'What Do You Expect From your Parking Spot?', 'Position', [100, 100, 300, 50 + 30 * numOptions1]);

% Create a ButtonGroup to enforce single selection
buttonGroup1 = uibuttongroup(Quality, ...
    'Position', [10, 40, 280, 30 * numOptions1], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions1
    uiradiobutton(buttonGroup1, ...
        'Text', options1{i}, ...
        'Position', [10, 30 * (numOptions1 - i), 250, 22]);
end

% Add "OK" button
okButton1 = uibutton(Quality, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(Quality));

% Wait for user interaction
uiwait(Quality);

% Get the selected option
selectedOption1 = buttonGroup1.SelectedObject.Text;

% Close the figure
close(Quality);

% Display the selected option
disp('Selected Option:');
disp(selectedOption1);
switch selectedOption1
    case 'I Want The Most Affordable Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'large' size and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'large' size and 'empty' status
     filteredData = data(strcmp(data.type, 'normal') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'large'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case 'I Want The Most Secure Spot'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'large' size and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'large' size and 'empty' status
     filteredData = data(strcmp(data.type, 'CCTV') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Display the selected spot
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'large'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
    break
end
end
    case  'I Want My Spot To Not Get Dirty'
while true
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Extract columns for filtering
    ids = data.id; 
    sizes = data.size; 
    types = data.type; 
    status = data.status;

    % Filter the data for 'large' size and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    
% Define the options
options2 = {'As Far As Possible', 'As Close As Possible', 'Does Not Matter'};
numOptions2 = numel(options2);

% Create the figure
fig2 = uifigure('Name', 'How Far Do You Want Your Spot To Be?', 'Position', [100, 100, 300, 50 + 30 * numOptions2]);

% Create a ButtonGroup to enforce single selection
buttonGroup2 = uibuttongroup(fig2, ...
    'Position', [10, 40, 280, 30 * numOptions2], ...
    'SelectionChangedFcn', @(~, event) disp(['Selected: ', event.NewValue.Text]));

% Create radio buttons for each option
for i = 1:numOptions2
    uiradiobutton(buttonGroup2, ...
        'Text', options2{i}, ...
        'Position', [10, 30 * (numOptions2 - i), 250, 22]);
end

% Add "OK" button
okButton2 = uibutton(fig2, ...
    'Text', 'OK', ...
    'Position', [100, 10, 100, 30], ...
    'ButtonPushedFcn', @(~, ~) uiresume(fig2));

% Wait for user interaction
uiwait(fig2);

% Get the selected option
selectedOption2 = buttonGroup2.SelectedObject.Text;

% Close the figure
close(fig2);

% Display the selected option
disp('Selected Option:');
disp(selectedOption2);

switch selectedOption2
    
    case 'As Far As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the maximum digit sum within the filtered data
    [~, maxIdxFiltered] = max(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    mostFurtherSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The most further spot is:');
    disp(mostFurtherSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', mostFurtherSpot.id, mostFurtherSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');


    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == mostFurtherSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = mostFurtherSpot.id; % Get the ID of the selected spot
    selectedType = mostFurtherSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  
    break

    case 'As Close As Possible'
        
    % Read the data from the file
    data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);

    % Filter the data for 'CCTV' type and 'empty' status
    filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Compute the sum of digits for each ID in the filtered data
    digitSumsFiltered = arrayfun(@(x) sum(str2double(regexp(num2str(x), '\d', 'match'))), filteredData.id);

    % Find the index of the minimum digit sum within the filtered data
    [~, maxIdxFiltered] = min(digitSumsFiltered);

    % Retrieve the row corresponding to the most further spot
    ClosestSpot = filteredData(maxIdxFiltered, :);

    % Display the result
    disp('The closest spot is:');
    disp(ClosestSpot);
    
    % Convert mostFurtherSpot to a string (e.g., spot ID and type)
    spotInfo = sprintf('Spot ID:%d, Type:%s', ClosestSpot.id, ClosestSpot.type{1});

    % Ask Customer To Proceed
    choice = menu(sprintf('%s\nIs Your Best Option For Now. Do you Want This Spot?', spotInfo), 'Yes', 'No');

    if choice == 1
    % Update the status of the most further spot in the original table
    data.status(data.id == ClosestSpot.id) = {'full'}; % Set the status to 'full'

    % Save the updated table back to the file
    writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',');

    % Display the selected spot
    selectedSpot = ClosestSpot.id; % Get the ID of the selected spot
    selectedType = ClosestSpot.type{1}; % Extract the type (string) from the cell
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);

    elseif choice == 2
    % Do nothing
    else
    disp('No selection made.');
    end  


    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end
    break
    
    case 'Does Not Matter'
        
    % Filter the data for 'large' size and 'empty' status
     filteredData = data(strcmp(data.type, 'Covered') & strcmp(data.status, 'empty') & strcmp(data.size, 'large'), :);

    % Display the filtered results
    disp(filteredData);

    % Get the number of available spots
    numSpots = height(filteredData);

    % Check if there are any available spots
    if numSpots == 0
    msgbox('Sorry, we could not find you any empty spots. Come back later 😔');
    return
    end

    % Create a dynamic message for the menu
    message = sprintf('Great, we have found %d spots for your car type. Choose one:', numSpots);

    % Create menu options dynamically with 'id' and 'type' in the format "id_type"
    options = strcat(string(filteredData.id), '_', string(filteredData.type));

    % Display the menu with the available spots
    spotIndex = menu(message, options{:});
    
    % Handle the user's selection
    if spotIndex > 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'large'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    end
    break
end
end
    otherwise
        time5 = msgbox('Alright,Whatever Costumer Says! 😊');
        uiwait(time5)
        % Ask if the user wants to check in another car
    another_check_in = menu('Do you want to check in another car?', 'Yes', 'No');
    
    if another_check_in == 2 % If the user chooses No, exit the loop
        time1 = msgbox('Thank you for using the parking system!');
        uiwait(time1);
        break;
    end
end
    end
    % Handle the user's selection
    if spotIndex > 0 && choice == 0
    % Extract the selected spot and type correctly
    selectedSpot = filteredData.id(spotIndex);
    selectedType = filteredData.type{spotIndex};  % Use curly braces to extract a value from a cell
    % Display the selected spot
    fprintf('You selected spot ID: %d of type: %s\n', selectedSpot, selectedType);
    % Find the row in the original data corresponding to the selected spot
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, 'large'));
    
    if ~isempty(rowIndex)
        % Update the status of the selected spot to 'full'
        data.status(rowIndex) = {'full'};
        
        % Save the updated data back to the file
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "full" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
    else
        disp('No spot selected.');
    end
        break
end
end
%% Car Check out
while true
    % Car Check-out Menu
    car_check_out = menu('Any Check outs?', 'Yes', 'No');
    
    switch car_check_out
        case 1
            % Read the data from the file
            data = readtable('C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'ReadVariableNames', true);
            
            % Filter for full spots
            filteredData = data(strcmp(data.status, 'full'), :);
            
            % Get the number of available full spots
            numSpots = height(filteredData);
            
            if numSpots == 0
                msgbox('No Full Spots Found');
                return;
            end
            
            % Create menu options for available spots
            message = sprintf('Great, we have found %d full spots. Choose one:', numSpots);
            options = strcat(string(filteredData.id), '_', string(filteredData.size), '_', string(filteredData.type));
            
            if numSpots > 1
                spotIndex = menu(message, options{:});
            else
                fprintf('There is only one spot: %s\n', options{1});
                spotIndex = 1;
            end
            
            if spotIndex > 0
                % Extract selected spot information
                selectedSpot = filteredData.id(spotIndex);
                selectedType = filteredData.type{spotIndex};
                selectedSize = filteredData.size{spotIndex};
                
                % Prompt for the number of days
                days_input = inputdlg('Enter number of days parked: ');
                days = str2double(days_input);
                
                % Calculate price based on size
                [static_price, daily_rate] = calculate_parking_price(selectedSize);
                
                % Calculate total price
                total_price = static_price + (daily_rate * days);
                
                % Display the result in a message box
                result_message = generate_price_breakdown(selectedSpot, selectedSize, selectedType, days, static_price, daily_rate, total_price);
                time4 = msgbox(result_message, 'Parking Fee Calculation');
                uiwait(time4)
                % Update parking spot status and save
                update_parking_spot_status(data, selectedSpot, selectedType, selectedSize);
                
                % Display thank you message
                time2 = msgbox('Thank you for using our parking system! Have a good day! 😊');
                uiwait(time2);
            else
                disp('No spot selected.');
            end
            
        case 2
            time3 = msgbox('Thank You And Have a good day! 😊');
            uiwait(time3);
            break;  % Exit the loop if 'No' is selected
    end
end
%% Price
function [static_price, daily_rate] = calculate_parking_price(selectedSize)
    switch lower(char(selectedSize))
        case 'small'
            static_price = 100;
            daily_rate = 20;
        case 'medium'
            static_price = 150;
            daily_rate = 30;
        case 'large'
            static_price = 200;
            daily_rate = 40;
        otherwise
            static_price = 0;
            daily_rate = 0;
    end
end

% Function to generate the price breakdown message
function result_message = generate_price_breakdown(selectedSpot, selectedSize, selectedType, days, static_price, daily_rate, total_price)
    result_message = sprintf(['Spot Information:\n' ...
                              'ID: %d\n' ...
                              'Size: %s\n' ...
                              'Type: %s\n' ...
                              'Days Parked: %d\n\n' ...
                              'Price Breakdown:\n' ...
                              'Static Price: $%d\n' ...
                              'Daily Rate: $%d x %d days = $%d\n' ...
                              'Total Price: $%d'], ...
                              selectedSpot, selectedSize, selectedType, days, ...
                              static_price, daily_rate, days, (daily_rate * days), ...
                              total_price);
end

% Function to update the parking spot status and save the updated table
function update_parking_spot_status(data, selectedSpot, selectedType, selectedSize)
    rowIndex = find(data.id == selectedSpot & strcmp(data.type, selectedType) & strcmp(data.size, selectedSize));
    
    if ~isempty(rowIndex)
        data.status(rowIndex) = {'empty'};
        writetable(data, 'C:\Users\Homelan\Desktop\ParkingSystem\parking_spots.txt', 'Delimiter', ',', 'WriteVariableNames', true);
        disp('Spot status updated to "empty" and file saved.');
    else
        disp('Error: Spot not found in the data.');
    end
end
