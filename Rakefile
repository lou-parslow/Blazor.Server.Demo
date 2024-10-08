require "raykit"

task :setup do
    run "dotnet new sln -n Lou-Parslow.Blazor.Server.Demo" unless File.exist? "Lou-Parslow.Blazor.Server.Demo.sln"
    run "dotnet new blazorserver -n Lou-Parslow.Blazor.Server.Demo -o src/Lou-Parslow.Blazor.Server.Demo" unless Dir.exist? "src/Lou-Parslow.Blazor.Server.Demo"
    run "dotnet sln add src/Lou-Parslow.Blazor.Server.Demo/Lou-Parslow.Blazor.Server.Demo.csproj"
    ["Microsoft.Identity.Web"].each do |package|
        run "dotnet add src/Lou-Parslow.Blazor.Server.Demo/Lou-Parslow.Blazor.Server.Demo.csproj package #{package}"
    end
end

task :build do
    run "dotnet build " \
        "src/Lou-Parslow.Blazor.Server.Demo/Lou-Parslow.Blazor.Server.Demo.csproj " \
        "--configuration Release"
end

task :deploy => [:build,:integrate,:push]

task :default => [:setup,:build] do
    puts "rake :deploy to push changed to cloud"
end