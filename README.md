# Keke-Protocol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

// lib/openzeppelin-contracts/contracts/token/ERC20/IERC20.sol

// OpenZeppelin Contracts (last updated v5.1.0) (token/ERC20/IERC20.sol)

/**
 * @dev Interface of the ERC-20 standard as defined in the ERC.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

// lib/openzeppelin-contracts/contracts/utils/Context.sol

// OpenZeppelin Contracts (last updated v5.0.1) (utils/Context.sol)

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function _contextSuffixLength() internal view virtual returns (uint256) {
        return 0;
    }
}

// lib/openzeppelin-contracts/contracts/access/Ownable.sol

// OpenZeppelin Contracts (last updated v5.0.0) (access/Ownable.sol)

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// lib/openzeppelin-contracts/contracts/token/ERC20/extensions/IERC20Metadata.sol

// OpenZeppelin Contracts (last updated v5.1.0) (token/ERC20/extensions/IERC20Metadata.sol)

/**
 * @dev Interface for the optional metadata functions from the ERC-20 standard.
 */
interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

// src/KEKEToken.sol

interface IKekeVRFBurn {
    function lastRandomHour() external view returns (uint256);
    function lastRandomBurnTime() external view returns (uint256);
}

contract KEKEToken is IERC20Metadata, Ownable {
    string private constant _name = "KEKE";
    string private constant _symbol = "KEKE";
    uint8 private constant _decimals = 18;

    uint256 private constant INITIAL_SUPPLY = 75_000_000 * 1e18;
    uint256 private constant MAX_SUPPLY = 300_000_000 * 1e18;
    uint256 private constant REBASE_SUPPLY = 225_000_000 * 1e18;
    uint256 private constant TOTAL_GONS = type(uint256).max - (type(uint256).max % MAX_SUPPLY);
    uint256 private constant REBASE_INTERVAL = 30 minutes;
    uint256 private constant REBASE_DELAY = 30 days;
    uint256 private constant REBASE_RATE = 182;
    uint256 private constant MAX_EPOCHS_PER_CALL = 5;

    uint256 private _totalSupply;
    uint256 private _gonsPerFragment;
    uint256 private _distributedRebase;
    uint256 public startRebaseTime;
    uint256 public lastRebaseTimestamp;
    bool public rebaseEnded;
    bool public taxActive;

    mapping(address => uint256) private _gonBalances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public immutable treasuryWallet;
    address public immutable charityWallet;
    address public immutable burnWallet;
    address public liquidityPool;
    address public vrfBurnModule;

    constructor(
        address _treasury,
        address _charity,
        address _initialOwner,
        uint256 _presaleEndTime
    ) Ownable(_initialOwner) {
        treasuryWallet = _treasury;
        charityWallet = _charity;
        burnWallet = 0x000000000000000000000000000000000000dEaD;

        startRebaseTime = _presaleEndTime + REBASE_DELAY;
        lastRebaseTimestamp = _presaleEndTime;
        taxActive = false;

        _totalSupply = INITIAL_SUPPLY;
        _gonsPerFragment = TOTAL_GONS / _totalSupply;
        _gonBalances[_initialOwner] = TOTAL_GONS;

        emit Transfer(address(0), _initialOwner, INITIAL_SUPPLY);
    }

    function name() public pure override returns (string memory) { return _name; }
    function symbol() public pure override returns (string memory) { return _symbol; }
    function decimals() public pure override returns (uint8) { return _decimals; }
    function totalSupply() public view override returns (uint256) { return _totalSupply; }
    function balanceOf(address account) public view override returns (uint256) {
        return _gonBalances[account] / _gonsPerFragment;
    }
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0) && spender != address(0), "Zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function setLiquidityPool(address _lp) external onlyOwner {
        require(_lp != address(0), "Zero LP address");
        liquidityPool = _lp;
    }

    function activateTax() external onlyOwner {
        taxActive = true;
    }

    function setVrfBurnModule(address _module) external onlyOwner {
        vrfBurnModule = _module;
    }

    function triggerBurnIfHourMatch() external {
        require(vrfBurnModule != address(0), "VRF not set");
        IKekeVRFBurn vrf = IKekeVRFBurn(vrfBurnModule);
        uint256 scheduledHour = vrf.lastRandomHour();
        uint256 lastCall = vrf.lastRandomBurnTime();
        require(block.timestamp >= lastCall + 30 days, "Not due yet");
        uint256 currentHour = (block.timestamp / 3600) % 24;
        require(currentHour == scheduledHour, "Wrong hour");
        _burn5Percent();
    }

    function _burn5Percent() internal {
        uint256 toBurn = (_totalSupply * 5) / 100;
        _totalSupply -= toBurn;
        _gonsPerFragment = TOTAL_GONS / _totalSupply;
        emit Transfer(address(this), burnWallet, toBurn);
    }

    function _transfer(address from, address to, uint256 amount) internal {
        _rebaseIfNeeded();
        uint256 gonAmount = amount * _gonsPerFragment;
        require(_gonBalances[from] >= gonAmount, "Insufficient");
        uint256 gonAfter = gonAmount;

        if (taxActive) {
            if (from == liquidityPool) {
                uint256 fee = amount / 100;
                uint256 gonFee = fee * _gonsPerFragment;
                _gonBalances[charityWallet] += gonFee;
                gonAfter -= gonFee;
                emit Transfer(from, charityWallet, fee);
            } else if (to == liquidityPool) {
                uint256 treasuryFee = amount / 100;
                uint256 burnFee = (amount * 2) / 100;
                uint256 gonTreasury = treasuryFee * _gonsPerFragment;
                uint256 gonBurn = burnFee * _gonsPerFragment;
                _gonBalances[treasuryWallet] += gonTreasury;
                _gonBalances[burnWallet] += gonBurn;
                gonAfter -= (gonTreasury + gonBurn);
                emit Transfer(from, treasuryWallet, treasuryFee);
                emit Transfer(from, burnWallet, burnFee);
            }
        }

        _gonBalances[from] -= gonAmount;
        _gonBalances[to] += gonAfter;
        emit Transfer(from, to, gonAfter / _gonsPerFragment);
    }

    function _rebaseIfNeeded() internal {
        if (rebaseEnded || block.timestamp < startRebaseTime) return;
        if (block.timestamp < lastRebaseTimestamp + REBASE_INTERVAL) return;

        uint256 epochs = (block.timestamp - lastRebaseTimestamp) / REBASE_INTERVAL;
        if (epochs > MAX_EPOCHS_PER_CALL) epochs = MAX_EPOCHS_PER_CALL;

        for (uint256 i = 0; i < epochs; i++) {
            if (_distributedRebase >= REBASE_SUPPLY) {
                rebaseEnded = true;
                break;
            }

            uint256 delta = (_totalSupply * REBASE_RATE) / 1_000_000;
            if (_distributedRebase + delta > REBASE_SUPPLY) {
                delta = REBASE_SUPPLY - _distributedRebase;
            }

            _totalSupply += delta;
            _gonsPerFragment = TOTAL_GONS / _totalSupply;
            _distributedRebase += delta;
        }

        lastRebaseTimestamp = block.timestamp;
    }
    receive() external payable {}
}

