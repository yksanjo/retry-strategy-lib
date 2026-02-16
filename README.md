# Retry Strategy Library

Advanced retry strategies for Node.js applications.

## Features

- **Exponential Backoff** - Classic exponential delay between retries
- **Full Jitter** - Random delay to prevent thundering herd
- **Decorrelated Jitter** - Better for high contention scenarios
- **Adaptive Retry** - Self-adjusting based on success/failure patterns
- **Circuit Breaker Aware** - Enhanced delays during failure cascades

## Installation

```bash
npm install retry-strategy-lib
```

## Usage

```typescript
import { 
  calculateExponentialBackoff, 
  ExponentialBackoffStrategy,
  createRetryStrategy,
  sleep 
} from 'retry-strategy-lib';

// Simple exponential backoff
const delay = calculateExponentialBackoff(0, 1000, 30000, 2);
await sleep(delay);

// Using the strategy class
const strategy = new ExponentialBackoffStrategy({
  maxRetries: 3,
  baseDelay: 1000,
  maxDelay: 30000,
  backoffMultiplier: 2,
  jitter: true
});

for (let attempt = 0; strategy.shouldRetry(attempt); attempt++) {
  const delay = strategy.getDelay(attempt);
  await sleep(delay);
  // try your operation
}
```

## API

### Functions

- `calculateExponentialBackoff(attempt, baseDelay, maxDelay, backoffMultiplier, jitter)` - Calculate delay
- `calculateFullJitterBackoff(attempt, baseDelay, maxDelay, backoffMultiplier)` - Full jitter delay
- `calculateDecorrelatedJitterBackoff(attempt, baseDelay, maxDelay, previousDelay)` - Decorrelated jitter
- `sleep(ms)` - Promise-based sleep
- `sleepWithAbort(ms, abortSignal)` - Sleep with abort support

### Classes

- `ExponentialBackoffStrategy` - Standard exponential backoff
- `AdaptiveRetryStrategy` - Self-adjusting retry strategy
- `CircuitBreakerAwareStrategy` - Circuit breaker aware retries

### Factory

- `createRetryStrategy(config, type)` - Create strategy by type

## License

MIT
