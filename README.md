# AI-Powered-Crypto-Scalping-Bot-for-Binance-BTC-USDT-SOL-USDT-
AI-Powered Crypto Scalping Bot for Binance (BTC/USDT &amp; SOL/USDT)
"""
✅ TEST SUITE
Test all components of the bot
"""

import logging
from binance_client import BinanceClient
from trading_strategy import TradingStrategy
from trade_logger import TradeLogger

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)


def test_binance_connection():
    """Test Binance API connection"""
    print("\n" + "="*60)
    print("🧪 TEST 1: Binance Connection")
    print("="*60)
    
        client = BinanceClient()
        
        if client.check_connection():
            print("✅ Binance API connection successful")
            
            # Get account balance
            balance = client.get_account_balance()
            if balance:
                print(f"✅ Retrieved account balance ({len(balance)} assets)")
                for asset, amounts in list(balance.items())[:3]:
                    print(f"   {asset}: {amounts['free']} (Free)")
            
            # Get current prices
            for symbol in ['BTCUSDT', 'SOLUSDT']:
                price = client.get_current_price(symbol)
                if price:
                    print(f"✅ {symbol}: ${price:.2f}")
            
            return True
        else:
            print("❌ Binance connection failed")
            return False
    
    except Exception as e:
        print(f"❌ Error: {e}")
        return False


def test_trading_strategy():
    """Test trading strategy"""
    print("\n" + "="*60)
    print("🧪 TEST 2: Trading Strategy")
    print("="*60)
    
    try:
        strategy = TradingStrategy()
        
        # Test recommendations
        for symbol in ['BTCUSDT', 'SOLUSDT']:
            print(f"\n📊 Analyzing {symbol}...")
            
            rec = strategy.get_trading_recommendation(symbol)
            if rec:
                print(f"   Price: ${rec['current_price']:.2f}")
                print(f"   Momentum: {rec['momentum']}")
                print(f"   Recommendation: {rec['recommendation']}")
                print(f"✅ Strategy analysis complete for {symbol}")
            else:
                print(f"⚠️ Could not get recommendation for {symbol}")
        
        return True
    
    except Exception as e:
        print(f"❌ Error: {e}")
        return False


def test_trade_logger():
    """Test trade logging"""
    print("\n" + "="*60)
    print("🧪 TEST 3: Trade Logger")
    print("="*60)
    
    try:
        logger_instance = TradeLogger()
        
        # Log test trades
        trade1 = logger_instance.log_trade('BTCUSDT', 80000, 80040, 40, 'CLOSED')
        if trade1:
            print("✅ Logged BTC trade")
        
        trade2 = logger_instance.log_trade('SOLUSDT', 195.50, 195.60, 0.10, 'CLOSED')
        if trade2:
            print("✅ Logged SOL trade")
        
        # Get statistics
        stats = logger_instance.get_statistics()
        print(f"✅ Total trades: {stats['total_trades']}")
        print(f"✅ Total profit: ${stats['total_profit']:.2f}")
        print(f"✅ Win rate: {stats['win_rate']:.1f}%")
        
        return True
    
    except Exception as e:
        print(f"❌ Error: {e}")
        return False


def run_all_tests():
    """Run all tests"""
    print("\n")
    print("╔════════════════════════════════════════════════════════════════╗")
    print("║          🧪 CRYPTO AUTO BOT - COMPREHENSIVE TEST SUITE         ║")
    print("╚════════════════════════════════════════════════════════════════╝")
    
    results = []
    
    # Test 1: Binance Connection
    results.append(("Binance Connection", test_binance_connection()))
    
    # Test 2: Trading Strategy
    results.append(("Trading Strategy", test_trading_strategy()))
    
    # Test 3: Trade Logger
    results.append(("Trade Logger", test_trade_logger()))
    
    # Print summary
    print("\n" + "="*60)
    print("📋 TEST SUMMARY")
    print("="*60)
    
    for test_name, result in results:
        status = "✅ PASSED" if result else "❌ FAILED"
        print(f"{test_name:<30} {status}")
    
    print("="*60)
    
    all_passed = all(result for _, result in results)
    if all_passed:
        print("\n🎉 All tests passed! Your bot is ready to use.\n")
    else:
        print("\n⚠️ Some tests failed. Please check your configuration.\n")
    
    return all_passed


if __name__ == "__main__":
    run_all_tests()
