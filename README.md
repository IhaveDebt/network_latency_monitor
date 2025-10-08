/**
 * Network Latency Monitor
 *
 * Simulates pinging a set of servers and logs latency metrics
 * with alerts when performance drops below acceptable thresholds.
 */

class LatencyMonitor {
  private targets: string[];
  private threshold: number;

  constructor(targets: string[], threshold = 100) {
    this.targets = targets;
    this.threshold = threshold;
  }

  async checkLatency() {
    for (const t of this.targets) {
      const latency = this.mockPing(t);
      const status = latency > this.threshold ? "⚠️ High latency" : "✅ Stable";
      console.log(`${t.padEnd(25)} ${latency.toFixed(2)} ms — ${status}`);
    }
  }

  private mockPing(_: string): number {
    return Math.random() * 200; // simulate ms latency
  }

  start(interval = 2000) {
    console.log("Starting latency monitor...");
    setInterval(() => this.checkLatency(), interval);
  }
}

if (require.main === module) {
  const monitor = new LatencyMonitor(["serverA.net", "db.cluster", "cdn.edge"], 120);
  monitor.start(1500);
}
