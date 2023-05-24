env "DB_ENGINE", "mongodb"

runner "TeamARunner", "ubuntu-latest"

transform "sleep" do |item|
  wait_time = item["arguments"][0]["value"]["value"]

  {
    "name": "Sleep for #{wait_time} seconds",
    "run": "sleep #{wait_time}s",
    "shell": "bash"
  }
end

transform "junit" do |item|
  test_results = item["arguments"].find{ |a| a["key"] == "testResults" }
  file_path = test_results.dig("value", "value")

  {
    "uses" => "actions/upload-artifact@v3",
    "with" => {
      "name" => "junit-artifact",
      "path" => file_path
    }
  }
end