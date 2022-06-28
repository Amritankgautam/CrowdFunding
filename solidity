// SPDX-License-Identifier:GPL-3.0
pragma solidity  0.8.12;

 contract CrowdFunding{
      mapping(address =>uint)  public contributors;
      address public manager;
      uint public minimumContribution;
      uint public target;
      uint public deadline;
      uint public raisedAmount;
      uint public noofContributors;

       struct Request{
           string description;
           address payable recipent;
           uint value;
           bool completed;
           uint noofVoters;
           mapping(address=>bool)voters;//consists of the address of the voters
                      
       }   
               
       mapping(uint=>Request)public requests;//mapping of multiple requests
       uint public numRequests;  //in mapping the increment thing is not possible though it is poosible in an array 
        




      constructor (uint _target, uint _deadline){
          target=_target;
          deadline=block.timestamp +_deadline;
          minimumContribution=100 wei;
          manager=msg.sender;
      }

        function sendEth() public payable{
            require(block.timestamp< deadline,"deadline has passed");
            require(msg.value>minimumContribution,"mimimum Contribution is not met");
            if(contributors[msg.sender]==0){
                noofContributors++;
            }
            contributors[msg.sender]+=msg.value;
            raisedAmount+=msg.value;
             
        }
        
  function getContractBalance() public view returns(uint){
            return address(this).balance;
         }
       function refund() public{
          require(block.timestamp>deadline && raisedAmount<target,"you are not eligible for refund");
          require(contributors[msg.sender]>0);
          address payable user=payable(msg.sender);
          user.transfer (contributors[msg.sender]);
          contributors[msg.sender]=0;
       }
      
       modifier onlyManager(){
           require(msg.sender==manager,"only manager can access this function");
           _;
       } 
       function createRequests(string memory _description,address payable _recipient,uint _value) public onlyManager{
           Request storage newRequest = requests[numRequests];
           numRequests++;
           newRequest.description=_description;
           newRequest.recipent=_recipient;
           newRequest.value=_value;
           newRequest.completed=false;
           newRequest.noofVoters= 0;
       }
        function voteRequest(uint _requestNo) public{
            require (contributors[msg.sender]>0,"ypo must be a contributor");
            Request storage thisRequest=requests[_requestNo];
            require(thisRequest.voters[msg.sender]==false,"you have already voted");
            thisRequest.voters[msg.sender]=true;
            thisRequest.noofVoters++;
        }
          function makePayment(uint _requestNo) public onlyManager{
              require(raisedAmount>=target);
              Request storage thisRequest=requests[_requestNo];
              require(thisRequest.completed==false,"the request has been completed");
              require(thisRequest.noofVoters > noofContributors/2,"majority does not support");
              thisRequest.recipent.transfer(thisRequest.value);
              thisRequest.completed=true;
          }





      }
