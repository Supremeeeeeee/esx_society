# esx_society
Need help.



Ok, so i am adding in a callsign option to esx_society but i am currently stuck at how to input the callsign to the database using esx_menu_dialog. Click on callsign then the esx_menu_dialog appears and imput whatever callsign and it saves to the database.

https://streamable.com/9zl3pt

![image](https://user-images.githubusercontent.com/59232670/125348149-c4e40280-e353-11eb-9b94-8944b0fefb68.png)

![image](https://user-images.githubusercontent.com/59232670/125348093-b3025f80-e353-11eb-97e1-24f8e07f311e.png)

Client side
---
      if data.value == 'callsign' then
        ESX.UI.Menu.Open(
          'dialog', GetCurrentResourceName(), 'callsign_',
          {
            title = _U('callsign')
          },
          function(data, menu)

            local args = callsign

            if args == nil then
              exports['mythic_notify']:DoCustomHudText('inform', 'Invalid Callsign')
              else
              menu.close()
              OpenEmployeeList(society)
              TriggerServerEvent('esx_society:setCallsign', callsign)
            end
          end,
          function(data, menu)
            menu.close()
              OpenEmployeeList(society)
          end
        )
      end
--
Server Side…
--
RegisterServerEvent('esx_society:setCallsign')
AddEventHandler('esx_society:setCallsign', function(source, args)
    local argString = table.concat(args, " ")
    MySQL.Async.execute('UPDATE users SET callsign = @callsign WHERE identifier = @source',{
		['@callsign'] = argString,
		["@source"] = GetPlayerIdentifiers(source)[1]
	},

        function(rowsChanged)
        TriggerClientEvent("output", source, "^2".. argString.. "^0")

    end)
end)
--
been at this all day and night but can’t seem to get it triggered.

Any help would be highly grateful
