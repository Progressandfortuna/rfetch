#!/usr/bin/env Rscript

# 1. Define the ASCII Art (R Logo)
logo <- c(
  "   RRRRRRRRRRR    ",
  "   RR       RR    ",
  "   RR       RR    ",
  "   RRRRRRRRRRR    ",
  "   RR RRR         ",
  "   RR   RRR       ",
  "   RR     RRR     "
)

# 2. Gather System Information
sys <- Sys.info()
user <- sys["user"]
host <- sys["nodename"]
os <- sys["sysname"]
kernel <- sys["release"]
r_version <- paste(R.version$major, R.version$minor, sep = ".")

get_uptime <- function() {
  if (file.exists("/proc/uptime")) {
    raw <- readLines("/proc/uptime", n = 1)
    uptime_sec <- as.numeric(strsplit(raw, " ")[[1]][1])
    hours <- floor(uptime_sec / 3600)
    minutes <- floor((uptime_sec %% 3600) / 60)
    return(sprintf("%dh %dm", hours, minutes))
  }
  return("Unknown")
}

get_mem <- function() {
  if (file.exists("/proc/meminfo")) {
    mem <- readLines("/proc/meminfo", n = 2)
    total <- as.numeric(gsub("[^0-9]", "", mem[1])) / 1024
    free <- as.numeric(gsub("[^0-9]", "", mem[2])) / 1024
    used <- total - free
    return(sprintf("%.0f MiB / %.0f MiB", used, total))
  }
  return("Unknown")
}

# 3. Format the Information Block
info <- c(
  sprintf("\033[1;34m%s\033[0m@\033[1;34m%s\033[0m", user, host),
  "-------------------",
  sprintf("\033[1;36mOS\033[0m: %s", os),
  sprintf("\033[1;36mKernel\033[0m: %s", kernel),
  sprintf("\033[1;36mUptime\033[0m: %s", get_uptime()),
  sprintf("\033[1;36mR Version\033[0m: %s", r_version),
  sprintf("\033[1;36mMemory\033[0m: %s", get_mem())
)

# 4. Print Side-by-Side
max_lines <- max(length(logo), length(info))
logo_width <- nchar(logo[1])
blank_logo <- paste(rep(" ", logo_width), collapse = "")

if (length(logo) < max_lines) {
  logo <- c(logo, rep(blank_logo, max_lines - length(logo)))
}
if (length(info) < max_lines) {
  info <- c(info, rep("", max_lines - length(info)))
}

cat("\n")
for (i in 1:max_lines) {
  cat(sprintf("  \033[1;34m%s\033[0m  %s\n", logo[i], info[i]))
}
cat("\n")
