require "raykit"

task :setup do
    run "dotnet new sln -n Lou-Parslow.Blazor.Server.Demo" unless File.exist? "Lou-Parslow.Blazor.Server.Demo.sln"
    run "dotnet new blazorserver -n Lou-Parslow.Blazor.Server.Demo -o src/Lou-Parslow.Blazor.Server.Demo" unless Dir.exist? "src/Lou-Parslow.Blazor.Server.Demo"
    run "dotnet sln add src/Lou-Parslow.Blazor.Server.Demo/Lou-Parslow.Blazor.Server.Demo.csproj"
end

task :build do
    run "dotnet build src/Lou-Parslow.Blazor.Server.Demo/Lou-Parslow.Blazor.Server.Demo.Server.csproj -o artifacts"
end